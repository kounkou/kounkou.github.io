# Binary search worst case complexity

## Introduction

Binary search is one of those algorithms that exibits a Mathematical property called the logarithm.
writing this first sentence, I am sure I may have lost a couple of readers already... but if you're still
reading this, you might have a chance to discover how this logarithm property emmerges from executing 
a Binary search algorithm. Hopefully, you might be able to apply this notion to understand other types of 
problems that exibit the same property.


## Binary search

To clearly understand the problem, imagine that you're given an array of integers A

```python
A = [1,2,3,4,5,6,7,8,9,9]
```

And using the binary search, we want to know what's the worst case for finding an element inside the array of integers given.
That question implies that you know how the binary search works.
So let me go ahead and do some refresher about the Binary search.
Suppose that we're searching for the number 5, to perform the binary search, we perform the following steps :

```python
[script 1]
        A
        |____ [1,2,3,4,5]             # 1
                   |
         [1,2] ____|____ [4,5]        # 2
           |               |
    [1] ___|___ [2] [4] ___|___ [5]   # 3

```

let's explain what the algorithm did step by step.

1. From the first the initial array, we chose the mid element (3) and saw that the element we are looking for (5)
is bigger than 3
2. so we took the second half of the array composed of [4,5]
3. we applied the same algorithm as in 1) and obtain the elemt 5 which is equal to the element we were looking for.

## Worse case time complexity

The above example is the perfect case of worse case binary search. And we want to know what exactly is the Time Complexity
of that worst case scenario. For me, the best way is to start by explaining exactly what we are looking for here.
The worse case time complexity here refers to a number for which we have the maximum height through the execution of the algorithm.
That height is obtained at a level where we have N children as on [script 1], and that number is a power of 2 as the initial is halved everytime.
We can see that at level 0 with have 1 parent, 
then at level 1 we have 2 children, etc.
That can be writen as :

```python
[script 1]
2^k = n
```

k being the highest level we want, to determine the worse case complixity of the binary search
can be obtained by using the log property.

```python
[script 2]
log2 n = k
```

## Final thoughts

So, from the above, we can deduce that the worse case time complexity of a binary search is actually the highest level
of the binary search tree. Which is :

```python
O(logn)
```


## References 
https://hackernoon.com/what-does-the-time-complexity-o-log-n-actually-mean-45f94bb5bfbf
