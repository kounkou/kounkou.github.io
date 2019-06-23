# Multi-threading


## Introduction

I have been talking to a colleague about multithreading in Python recently, but didn't realize the subtle traps of using
multithreading the Python... To save the face, I decided to really deep dive into Python multithreading and 
try as hard as I can to understand what this Python feature really is without any assumptions from my c++ background.
Multi-threading is the art for decoupling sequentially independent tasks. In this post, I will focus on 2 of the possibilities
offered by Python to perform multithreading computations. The first one is the basics module threading.
Coming from c++, threading is the most intuitive module available in order to perform multithreaded computations.
It offers basic primitives to create threads, start them and join or detach them.
Multiprocessing is more complex, and we'll learn about that module during the course of this post.

## Before talking about multithreading

Any serious post talking about multithreading in Python should talk about the Global Interpretor Lock (GIL).
So, let's talk about the GIL a little bit before digging into the understanding of multithreading technics.
The GIL is a Python solution to avoid possible race conditions or deadlocks possibly resulting from trying to fix the reference counting mecanism.
The GIL states that each Python bytecode shall only run inside **One** thread at a time. This means that a lock is set before executing an interpreter, and a release is done after the interpretor has finished executing the commands.
The GIL has doesn't drop performances on I/O bound applications, but does drop performances on CPU bound applications.

Now that we understand how the GIL works, we can see that a possible solution to solve above limitations.

## Solution to the GIL limitations

- multiprocessing
  Because each process creates its own interpreter, creating multiprocesses instead of multithreads will solve the problem
  stated above.
  Python provide a complete multiprocessing module

## References 

https://docs.python.org/2/tutorial/stdlib2.html#multi-threading

https://realpython.com/python-gil/
