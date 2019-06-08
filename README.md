# A journey through algorithms

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
To perform the binary search, we will perform the following steps :

```python
[to improve]
A
|____ [1,2,3,4,5]
           |____ [1,2]
                   |____ ...
           |____ [4,5]
                   |____ ...
|____ [6,7,8,9,9]
           |____ [6,7]
                   |____ ...
           |____ [9,9]
                   |____ ...
```
In other words, you're dividing the initial array to half size arrays if the element you're looking for
is either side of the initial array.
