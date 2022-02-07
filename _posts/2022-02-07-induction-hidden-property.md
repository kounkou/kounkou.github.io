# Induction hidden property

## Introduction

In my previous article called [Mathematical proofs](https://kounkou.github.io/2022/02/03/mathematical-proofs.html), I discussed 3 of the most powerful proof techniques.
One of them holds a secret property, a property that for some reasons makes a connection with my next article.
In this article, I will walk you though the Induction again, and introduce another notion that will unlock another level of understanding of Induction.


## A hidden property for induction

As discussed in the previous article, Induction is proven following 2 cases. The first case, also called base case or basis, proves the statement for some initial condition.
Let's take an example to illustrate this property.

Suppose you want to prove Fibonacci sequence is correct by induction, defined by : 


`fibonacci(n) = fibonacci(n-1) + fibonacci(n-2)`

The first step would then be to set some base case(s) : 

`fibonacci(0) = 0`
`fibonacci(1) = 1`

The next step would then be to prove that if fibonacci statement holds for a number n, it must hold for a number n+1.

`fibanacci(n+1) = fibanacci((n+1)-1) + fibonacci((n+1)-2)`

With a variable change `n -> N` still holds

`fibanacci(N) = fibanacci((N)-1) + fibonacci((N)-2)`

But wait a minute, what we just defined here, is exactly 

```
  fibonacci(0) = 0
  fibonacci(1) = 1
else  
  fibonacci(n) = fibonacci(n-1) + fibonacci(n-2)
```

Let's put it inside a function :

```
int fibonacci(int n) {
  if (n == 0) return 0;
  if (n == 1) return 1;
  
  return fibonacci(n-1) + fibonacci(n-2);
}
```

Wait a moment... ! Did we just write a `recursive` function while we were trying to prove fibonacci by `Induction` ?!
Well yes, that is correct. So with this in mind, I will now provide you with the Recursion definitions that should be kept in mind when dealing with 
Inductions.

```
Mathematical induction can be used to prove results about recursively defined sequences and functions. 
Structural induction is used to prove results about recursively defined sets.
```


## Final thoughts

Why is this property important ? You would ask... Well, the same way Einstein proved Energy equals Mass through the famous <img src="https://latex.codecogs.com/svg.image?E&space;=&space;mc^2" title="E = mc^2" />, it is important to understand 
Recursion has the same nature as Induction. Bipolarism over those 2 properties is the source of misunderstanding of one class of Optimization problems, called Dynamic
Programming. This concludes our brief article, I hope you enjoyed it!

## References 

- [Induction](https://en.wikipedia.org/wiki/Mathematical_induction)
- [Mass–energy equivalence](https://en.m.wikipedia.org/wiki/Mass%E2%80%93energy_equivalence)
