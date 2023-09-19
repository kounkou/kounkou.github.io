
# Loop invariant Proof

<img width="1000" height="450" alt="Screenshot 2023-09-18 at 11 55 48 PM" src="https://github.com/kounkou/kounkou.github.io/assets/2589171/5d2e9ba1-5a18-4599-8c46-0923a3430bc1">
Prevost Island

## Introduction

Some algorithms are so simple that, most people wouldn't look bebeath the surface.
I was reading a document online about sorting algorithms, and a very simple question was asking
to demonstrate how that sorting algorithm would produce the expected result, in short, produce a list of sorted elements.
I think that question is profound. It asks the question of our true understanding of how the algorithm produces the correct output.
In the following section, we will first expose an easy sorting algorithm, you've guessed it, a buble sort... and 
demonstrate that applying instructions on the container will produce the expected result following a method called loop invariant.

## Buble sort

I guess no introduction is needed for the buble sort here... but in case you're short in memory or new to programming,
let me describe how the buble sort algorithm works.
As the name suggests, Buble sort will sort a list of elements (Integers in our case), but moving the bigger elements to the end or the beginning of the list like bubles going up in a bottle of soda.
Here is an implementation of the buble sort in Python. yeah,... I am in love with Python these days.

```python

# Buble sort trivial method
def buble_sort(A) :
   for i in range(len(A) - 1):
      for j in range(len(A) - 1):
         if A[j + 1] < A[j]:
            A[k + 1],  A[j] = A[j], A[j + 1]
            
if __name__ == '__main__':
 
   A = [0, 2, 4, 8, 0, 1, 0, 0, 0, 0, 1, 2, 3]
   buble_sort(A)
   
   # print the resulting sorted array
   print(A)
```

The above code produces  :

```python
[0, 0, 0, 0, 0, 0, 1, 1, 2, 2, 3, 4, 8]
```

## Loop invariant

To prove that the code produces the right result on top of displaying the right result, we're going to use the loop invariant method. Loop invariant are a powerful tool to validate programs by stating assertions on the program, before, during and after the execution of the application. Please have a look at the references for more information.
To demonstrate that the buble sort produces the right result, let's describe the proof in 3 steps :

- #### Initialization
  Before starting any loop in the buble sort, elements in the range A[0] to A[i] are made of {0} elements.
  By convention, 0 elements are sorted. Notice that before starting a loop i = 0
  This implies that the invariant rule is naturally established here.
  
- #### Maintenance
  During the course of the execution, elements going from A[0] to A[i] will become the smallest elements and smaller than elements in the sub array going from A[i] to A[n]
  We can notice that with the given code, an invariant is maintained on the left side of the element located at position i
  
- #### Termination
  The last step is a conclusion that is obvious here... indeed, at the end of the loops, i is equal to the size of the array - 1, len(A) - 1 being the last position.
  This means that from the Maintenance step, all elements from A[0] to A[len(A) - 1] are then sorted. Meaning all elements in the array !


## Final thoughts

We just demonstrated that the result of executing the buble sort is a sorted array. I hope this article will help you put some logic on an algorithm as simple as the buble sort and much more !


## References

https://en.wikipedia.org/wiki/Loop_invariant

http://ifac.univ-nantes.fr/IMG/pdf/1er_ordre_bis.pdf
