---
layout: post
title: "Writing a Packet Parser with Packet Sockets in Linux"
date: 2018-06-15
categories: Network
---
As someone who works with packets on a daily basis, a summary about its inner structures and programming interfaces is long overdue. This post is intended to be practical rathr than through, as I will be skipping over things which are (IMO) less significat from a programmer's perspective. (No non-ethernet II and non-IP traffic will be discussed; no preamble or SFD on the ethernet frame)

## The Big Picture
![packet encapsulation](/assets/tcpip.png)
>> image from http://www.tcpipguide.com/free/t_IPDatagramEncapsulation.html

## The Plan
# Step1. Socket Creation
If you've done socket programming before, chances are you've seen code looking like `socket.socket(socket.AF_INET, ...)` somewhere. What it does is to create an IPv4 socket, where `AF_INET`is the **socket family** field, which tells the OS that our socket is intended to communicate with IPv4 addresses. That way, the kernel would then manage lower-level headers for us (automatically generate L2 headers when `send`ing packets, strip away L2 headers when `receive`ing packets). Because here we want to include the ethernet header information, we have to opt for something else. As it turns out, that something else is called **packet socket**. From [man 7 packet](http://man7.org/linux/man-pages/man7/packet.7.html), "**Packet sockets are used to receive or send raw packets at the device driver (OSI Layer 2) level.**"

![raw socket](/assets/raw_socket.jpg)
>> image from https://opensourceforu.com/2015/03/a-guide-to-using-raw-sockets/

The second argument we need to provide is the **socket type**. Also from the man page, when you create a packet socket, the type field is “**either `SOCK_RAW` for raw packet including the link-level header or `SOCK_DGRAM` for cooked packets with the link-level header removed**”. Since we want to dig into the layer 2 header, the second filed needs to be `SOCK_RAW`.

The third argument is the **protocol** field. Usually there is only one supported protocol given the socket family and type, so this field can be omitted. Since we are working with raw sockets and want to capture all protocols, the special value `0x0003` is used.

# Step2. Bind()?
A Bind() operation is unnecessary here. When working with packet sockets, by default all packets of the specified protocol will be passed to the socket(incoming and outgoing, on all interfaces). If we only want packets from a specific interface or address, we can use `bind` as a filter.

# Step3. Recvfrom() and Parsing

## A Quick Example 

# Ethernet Header:
```
bytes:   0 1 2 3 4 5      6 7 8 9 10 11     12 13
        +----------------+-----------------+------+
        | Dest.MAC       | Src.MAC         | Type |
        +----------------+-----------------+------+
```

* **Dest.MAC:** self-explanatory
* **Src.MAC:** self-explanatory
* **Type:** For value >= 1536(Ethernet II), this specify the L3 protocol (IPv4, IPv6, ARP). For value <= 1500(Ethernet 802), this field is the length of the header.

# The code (tested in Ubuntu , Python 3.6)
{% highlight python %}
import socket, sys, struct

eth_len = 14 

def print_mac(mac_string):
    print(':'.join('%02x'% b for b in mac_string))

try:
    s = socket.socket(socket.AF_PACKET, socket.SOCK_RAW, socket.ntohs(0x0003))
except socket.error as e:
    print('Socket creation failed.', e)
    sys.exit()

while True:
    data, addr = s.recvfrom(65565)      
    eth_header = data[:eth_len]
    eth_header = struct.unpack('!6s6sh', eth_header)

    eth_proto = socket.ntohs(eth_header[2])
    dest_mac = data[0:6]
    src_mac = data[6:12]

    if eth_proto == 8:  # IP 
        pass
{% endhighlight %}

# IP Header
# TCP Header
# UDP Header



Reference:
>> http://man7.org/linux/man-pages/man7/packet.7.html
>> https://linux.die.net/man/2/socket
>> https://stackoverflow.com/questions/1593946/what-is-af-inet-and-why-do-i-need-it


