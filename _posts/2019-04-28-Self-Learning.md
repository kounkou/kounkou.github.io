---
layout: post
title: "How I designed the self-learning module for firewall "
date: 2019-04-28
mathjax: true
categories: Network
---
# Introduction
Some time ago I was asked to develop a "self-learning" module for the company's iptables-based firewall product. The motive is that, since our firewall adopts a white-list policy, after installation customers have to manually set up many rules to allow existing network traffic to pass through unaffected. This is both time-consuming and sometimes impractical, as the user may not know much about the underlying network.

After some thinking, I came up with an idea: use raw sockets to sniff all traffic, classify and store them in a hashtable, and send the results to user through a FIFO for further processing (the user can send a signal to trigger such an action). After the user picks what connections he wishes to allow through back to the firewall, we generate corresponding iptables rules.

# The hidden problem
Everything sounds good so far, except for how to classify network traffic. The most obvious answer is to use a tuple of ``(protocol, src_ip, dst_ip, src_port, dst_port)`` for every packet, but this approach is hopelessly flawed: in a client-server model, when the client tries to connect to a server, it usually uses a randomly-generated port number. So if a client issues ``n`` seperate connections to a server, it might use n different port numbers, and we would in turn collect ``n`` different tuples. Since our end goal is to generate iptables rules, not only will this approach flood the rules chain, it also fixes the source port, so another connection issued by the client with a previously unseen port number would not be matched by the rule and thus dropped.

A better try is using ``(protocol, clientIP, serverIP, serverPort)`` tuple for each packet. Given that, we could then add a rule like ``iptables -A LEARNED_RULES -p protocol -s clientIP -d serverIP --dport serverPort -m state --state ESTABLISHED -j accept`` to iptables and be happy everafter. But how do we know which port number belongs to the server? Normally we could deduce it from identifying the server's external IP, but in my particular case, both the client and server sit in the same network and there is no easy way to tell them apart.

# The key insight
After some further thinking, I came to the conclusion that without assuming too much about the network itself, **the only way to identify the server's port is by number of occurrences**. Since the client's port may change each time and the server's port stays the same, if we collect and count all the ``(port1, port2)`` from each packet between a fixed ``(protocol, IP1, IP2)``, whichever number that appears more times overall is most likely the server port.

# The class
The entire problem is obviously data-oriented, so a dedicated class seems appropriate. 

```python
class Connection(object):
	def __init__(self, proto, sip, dip, sport, dport):
		self.proto = proto
		self.sip = sip
		self.dip = dip
		self.sport = sport
		self.dport = dport
	def getports(self):
		return (self.sport, self.dport)
```

Some extra care is required to make things work. First, since communication is often bi-directional, we need to make sure that (protocol, IP1, IP2) and (protocol, IP2, IP1) points to the same entry in the hashtable.

```python
def reverse(self):
	return Connection(self.proto, self.dip, self.sip, self.dport, self.sport)
```

Second, as discussed before, we would like each ``key`` to logically represent a **unique** ``(protocol, IP1, IP2)`` tuples and the corresponding ``value`` to represent **all** the ``(port1, port2)`` tuples . But the default class instance uses all the fields in ``__init__()`` for hashing and indexing in the dictionay. To change the default behaviour, we need to define our own ``__hash__()`` and ``__eq__()`` methods to only include the fileds that constitues a unique key.

```python
def __hash__(self):
	return hash((self.proto, self.sip, self.dip))
def __eq__(self, other):
	if not isinstance(other, type(self)):
		return False
	return self.proto == other.proto and self.sip == other.sip and self.dip == other.dip
```

# The flow
With our class ready, we can go ahead and classify connections. Only a fraction of the entire program is provided here for simplicity.

{% highlight python %}
signal.signal(signal.SIGUSR1, signal_handler)   
tracked = {}        
s = socket.socket(socket.AF_PACKET, socket.SOCK_RAW, socket.ntohs(0x0003))

while(true):
    packet = s.recvfrom(65565)
    conn = Connection(dissect_header(packet))

    if conn in tracked:
        tracked[conn].append(conn.getports())       # if seen before, append ports to existing list
    elif conn.reverse() in tracked:
        tracked[conn.reverse()].append(conn.reverse().getports())   # take care of the other direction
    else:
        tracked[conn] = [(conn.getports())]         # if not seen before, create new entry in the dictionary
{% endhighlight %}

In the above program, we sniff all packets from a raw socket, dissect the headers and generate corresponding ``Connection`` instances. We then store these instances in the ``tracked`` dictionary as keys (but only protocol, source IP and destination IP are used for hashing/indexing), and let the values be a list of all the port pairs that occured between these IPs. For example, after some time the content of trakced could look like this:

```python
{Connection('tcp', '10.1.3.1', '10.1.3.6', 12345, 102):[(12345, 102), (12345, 102),(12346, 102)],
 Connection('udp', '10.1.3.2', '10.1.3.25', 23245, 200):[(23245, 200), (23248, 200),
...
}
```
Just to illustrate one more time, suppose our first captured packet is a TCP request from IP=10.1.3.1, port=12345 to IP=10.1.3.6, port=102, we would create a new entry in the dictionary as ``Connection('tcp', '10.1.3.1', '10.1.3.6', 12345, 102):[(12345, 102)]``. Next, suppose we captured a reply from the server, an instance of ``Connection('tcp', '10.1.3.6', '10.1.3.1', 102, 12345)`` is created. Because our code handles the other direction, it is indexed to the same entry and the value would become ``[(12345, 102), (12345, 102)]``. Suppose after a while the client issues another tcp request, this time from the 12346 port, an instance of ``Connection('tcp', '10.1.3.1', '10.1.3.6', 12346, 102)`` is created. Because we redefined the class's ``__hash__``  methods, this instance would again be index to the same entry, updateing the value to ``[(12345, 102), (12345, 102), (12346, 102)]``.



Then, upon receiving a signal from the user, we take a deep look into each entry, count the total occurrence of the ports and decides which one is the server port. Notice that we have to traverse the entire list and deduce server port of each port pairs, instead of just using the port number with most occurrences, because there could be more than one server program at an IP.

{% highlight python %}
def signal_handler(signum, frame):
    for conn in tracked:
        ports = tracked[conn]	# list of port tuples
        sport_count = Counter(elem[0] for elem in ports)
        dport_count = Counter(elem[1] for elem in ports)
			
        for pair in ports:
            sport = pair[0]
            dport = pair[1]
            if sport_count[sport] > dport_count[dport]:
                # sport is the server port
            else:
                # dport is the server port
{% endhighlight %}

There is one more case where there are only two different ports and they appear exactly the same number of times. This happens when our sniffer only captured one (continuous) connection between a client and server. Unfortunately we cannot decide who is the server based on traffic alone so some other methods must be used, like picking the smaller number.

































 
