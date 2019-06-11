
# Decorator in Python

## Introduction

To be honest, I didn't think I would have to write a post talking about Python. Coming from a complicated language as C++,
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
Instead, let's understand what's going on when calling a decorator function.

