# Multi-threading

<img width="1000" height="450" alt="Screenshot 2023-09-18 at 11 50 09 PM" src="https://github.com/kounkou/kounkou.github.io/assets/2589171/cbe8ecc9-e3c3-4635-a8a7-1e05acf2d0d7">
Diosso

## Introduction

I have been talking to a colleague about multithreading in Python recently, but didn't realize the subtle traps of using
multithreading with Python... To save the face, I decided to really deep dive into Python multi-threading and 
try as hard as I can to understand what this Python feature really is without any assumptions from my c++ background.
multi-threading is the **art** for decoupling sequentially independent tasks. In this post, I will focus on 2 possibilities offered by Python to perform multi-threading computations. The first one is possible with the use of the basics module threading. Coming from c++, the threading module is the most intuitive module available in order to perform multithreaded computations. It offers basic primitives to create threads, start them and join or detach them. Here is an example code with multithreading and lock/release locks :

```python
import threading                                                                                                                  
import time
from random import randint

# critical section
RESOURCE = 0 

mutex = threading.Lock()

def modify_resource(val):
   print('working on ', threading.current_thread().name)
   time.sleep(randint(0, 10))
   mutex.acquire(True)
   print('acquire a lock... from ', threading.current_thread().name)
   mutex.release()
   print('release a lock... from ', threading.current_thread().name)

if __name__ == '__main__':
   arr = []
   for i in range(10):
      arr.append(threading.Thread(target = modify_resource, args = (i,)))
   for i in range(10):
      arr[i].start()
   for i in range(10):
      arr[i].join()

```

Multiprocessing is more complex, but simple to use and we'll learn about that module during the course of this post.

## Before talking about multithreading

Any serious post talking about multithreading in Python should talk about the Global Interpretor Lock (GIL).
So, let's talk about the GIL a little bit before digging into the understanding of multithreading technics.
The GIL is a Python solution to avoid possible race conditions or deadlocks resulting from trying to fix the reference counting problem.
The GIL states that each Python bytecode shall only run inside **One** thread at a time. This means that a lock is set at the beginning of the interpreter execution, and a release is done after the interpretor has finished executing the command.
The GIL has some specificities. It doesn't drop performances on I/O bound applications, but does drop performances on CPU bound applications.


## Solution to the GIL limitations

- with multiprocessing
  Because each process (instead of each thread) creates its own interpreter, creating multiprocesses instead of multithreads will solve the problem stated above.
  Here is aa basic example of multiprocessing using the multiprocess module and a Pool of workers.
  
```python
from multiprocessing import Pool

def f(x):
   return x * x
   
if __name__ == '__main__':
   p = Pool(processes = 4)
   print(p.map(f, [1, 2, 3])
```

The above code will create a Pool of workers.
And for each element provided to the pool of workers, the process will compute the square value and return it.

## Final thoughts

Processes represent for me the ultimate solution to parallelism with Python. Each process is executed in its corresponding interpreter, with it's own lock protection with respect to the GIL.


## References 

https://docs.python.org/2/tutorial/stdlib2.html#multi-threading

https://realpython.com/python-gil/
