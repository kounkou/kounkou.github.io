---
layout: post
title: "A Closer Look At CPython GIL"
date: 2019-05-31
categories: Python
---
# Introduction
I've always had a rough idea of what the GIL in Python does, but never delved deeper into it. Coming from a system programmer background, I thought I knew enough about locks already to not mess up in an "easy" language like Python. Well, something happended this week (which I will skip to save some face), and I realized my assumptions simply aren't true.  

Notice that GIL isn't present in all implementations of Python. For this post, I will only be talking about CPython.

# Why GIL
To understand GIL, it's helpful to learn why GIL is introduced in the first place. There are actually a number of reasons, but the main one is to safely implement **reference counting** which Python uses for memory management. 

In Python, every variable is a reference to an object in memory. For example, the statement ``var = 10`` just adds a reference named ``var`` to the object ``10``. It also increase the reference count of ``10`` by 1. If we reassign ``var`` to something else, the reference count of ``10`` decreases by 1 accordingly. When the refernece count of an object becomes 0, Python deallocates it from memory.

This is an classic case of race condition, since ``increase by 1`` and ``decrease by 1`` are not atomic operations on most platforms. So a lock is needed to prevent faulty updates in a multithreaded envirounment. One way is to have a lock for every object, but multiple locks could lead to deadlock and avoiding it requires code complexity. For speed and simplity, CPython uses a **global interpretor lock** to make sure that only one thread can run Python at a time.

# What Does GIL guarantee
This is the part that confused me so I will treat it with more details. When we run a Python program, there are two steps involved: 

1. Python code is compiled into something called **bytecode** 
2. CPython interpretor (a program itself) executes on these bytecodes. 

**The GIL is implemented in the CPython interpretor and it guarantees that only one thread executes Python bytecodes at any moment.**

To repeat, the GIL is a lock for your interpreter, not your code.

# When Does GIL Drop
When running a multithreaded Python program, the CPython interpreter periodically drops the GIL, switch to another thread, and acquires the GIL again before running the new thread. The thread switch can happen in two ways:

1. **Cooperative**: when the current thread is sleeping or waiting for I/O (i.e. not actualy executing bytecode). 
```
import requests
r = request.get("www.google.com")   # drops GIL
```
2. **Preemptive**: the interpretor could also decides that the current thread has been running for long enough and switch to another thread without warning. The precise condition is implementation-specific and could change, so you will have no control over it (In Python2 a switch happens after 1000bytecodes, in Python3 after 15 milliseconds). 

# Thread-safety in Python
Becasue of preemptive switches, a thread may be interrupted in the middle of executing a series of bytecodes. If these bytecodes modifies a shared resouce, race condition can occur. 

Thus in Python, what count as a **thread-safe operation** depend on the underlying bytecode. But the translation from Python code to bytecode is a constant work in progress and may change from release to release. So despite [the list](https://docs.python.org/3/faq/library.html#what-kinds-of-global-value-mutation-are-thread-safe) of automic opeartions, you should always use a lock when modifying a shared resouce to be future proof. 

# An Example:
{% highlight python %}
import requests
import threading

d = {'success':0, 'failure':1}
lock = threading.Lock()

def make_request():
    try:
        r = requests.get("http://localhost:12345/") # drops GIL, perfect for multithreads
        if r.status_code == 200:
            with lock:              # acquires lock
                d['success'] += 1
        if r.status_code == 500:
            with lock:              # acquires lock
                d['failure'] += 1
    except e:
        print("something went wrong")

threads = []
for _ in range(100):
    t = threading.Thread(target=make_request)
    threads.append(t)
    t.start()

for thread in threads:
    thread.join()

print(d)
{% endhighlight %}

# Final Note
* On my machine the above code could still produce consistent answer without locks in Python 3.6 but not in Python 2.7, because of the bytecode difference. But you should never rely on it. When modifying a shared resouce in a multithreaded environment, always use locks.

* Some libraries and C extensions can also be written to release the GIL and achieve true multithreading. 

# Reference
> https://opensource.com/article/17/4/grok-gil \\
> https://docs.python.org/3/faq/library.html#what-kinds-of-global-value-mutation-are-thread-safe
