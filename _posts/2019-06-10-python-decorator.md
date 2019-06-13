
# Decorator in Python

## Introduction

To be honest, I didn't think I would have to write a post talking about Python. Coming from a complicated language such as C++,
Python looks as an executable pseudocode language. But the more I look into the language, the more I see the beauty of the Python language. So I decided to write this little post about a feature that most Python scripters barely touch, but that I find extremely well made.

## Decorator structure

Let's start with the structure of the Python decorator before analyzing it

```python
from time import time

def func(f):
   print('f is located : ', id(f))
   def wrapper(x):
      print('inside the wrapper function')
      before = time()
      result = f(x)
      after  = time()
      print(result, ' in ', (after - before) / 1000)
      return result
   return wrapper
   
@func
def square(x):
   return x * x
   
if __name__ == '__main__':
   square(3)
```

## Analyzing the structure

The high level structure of the decorator is essentially wrapping up a function inside another function to be able to use the capacity to use the syntax **@func**
Instead, let's understand what's going on when calling a decorator function using a magic tool called dis.

```python
>>> dis.dis(func)
  2           0 LOAD_CLOSURE             0 (f)
              3 BUILD_TUPLE              1
              6 LOAD_CONST               1 (<code object wrapper at 0x7fa8d1645e30, file "<stdin>", line 2>)
              9 MAKE_CLOSURE             0
             12 STORE_FAST               1 (wrapper)

  8          15 LOAD_FAST                1 (wrapper)
             18 RETURN_VALUE        
>>> 
```

Pretty easy right, the func will create a python closure, and just return that created closure.
If you were looking for the all instructions executed inside the func, have a look at the dis below
it's when calling the square function that things get executed in a symphony !

```python
>>> dis.dis(square)
  3           0 LOAD_GLOBAL              0 (time)
              3 CALL_FUNCTION            0
              6 STORE_FAST               1 (before)

  4           9 LOAD_DEREF               0 (f)
             12 LOAD_FAST                0 (x)
             15 CALL_FUNCTION            1
             18 STORE_FAST               2 (result)

  5          21 LOAD_GLOBAL              0 (time)
             24 CALL_FUNCTION            0
             27 STORE_FAST               3 (after)

  6          30 LOAD_FAST                2 (result)
             33 LOAD_CONST               1 (' in ')
             36 LOAD_FAST                3 (after)
             39 LOAD_FAST                1 (before)
             42 BINARY_SUBTRACT     
             43 LOAD_CONST               2 (1000)
             46 BINARY_DIVIDE       
             47 BUILD_TUPLE              3
             50 PRINT_ITEM          
             51 PRINT_NEWLINE       

  7          52 LOAD_FAST                2 (result)
             55 RETURN_VALUE        
>>> 
```

## Final thoughts

What we've learnt from Python decorators is that they consist in encapsulating a function with a closure by adding instructions we want to execute before and after the main purpose function. We **decorate** the function with our code.
Then from a dedicated function routine, we execute the wrapped function.

## References 

https://docs.python.org/2/library/dis.html


