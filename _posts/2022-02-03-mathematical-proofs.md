# Mathematical Proofs

## Introduction

In this Blog post, I am discussing my understanding of my 3 most powerful Mathematical Proof strategies.


## Induction

Mathematical induction is a prood technic, that demonstrates that a statement
holds for all natural numbers N (0, 1, 2, 3, ...)
To prove a statement by induction, there are 2 steps to follow :

1. Prove that the statement holds for the Base case.
2. Prove that the statement holds for the case k + 1.

Example :

In this example we want to prove that the sum of n integers is 


<img src="https://latex.codecogs.com/svg.image?\sum_{1}^{k}&space;n&space;=&space;n&space;*&space;(n&space;&plus;&space;1)&space;/&space;2" title="\sum_{1}^{k} n = n * (n + 1) / 2" />

Proof : 

1. Prove that the statement holds for the Base case. 

<img src="https://latex.codecogs.com/svg.image?P(n)=0&plus;1&plus;2&plus;...&plus;n&space;=&space;n*(n&plus;1)/2&space;" title="P(n)=0+1+2+...+n = n*(n+1)/2 " />
<img src="https://latex.codecogs.com/svg.image?P(0)=0&plus;1&plus;2&plus;...&plus;0&space;=&space;0*(0&plus;1)/2&space;=&space;0&space;" title="P(0)=0+1+2+...+0 = 0*(0+1)/2 = 0 " />
<img src="https://latex.codecogs.com/svg.image?P(k&plus;1)=0&plus;1&plus;2&plus;...&plus;k&plus;1&space;=&space;(k&plus;1)*(k&plus;1&plus;1)/2&space;=&space;(k^{2}&space;&plus;&space;3k&space;&plus;&space;2)/2&space;" title="P(k+1)=0+1+2+...+k+1 = (k+1)*(k+1+1)/2 = (k^{2} + 3k + 2)/2 " />
<img src="https://latex.codecogs.com/svg.image?P(k&plus;1)=(k&plus;1)((k&plus;1)&plus;1)/2&space;=&space;P(k&plus;1)&space;" title="P(k+1)=(k+1)((k+1)+1)/2 = P(k+1) " />

If we change k+1 for K, we obtain 

<img src="https://latex.codecogs.com/svg.image?P(K)=(K)((K)&plus;1)/2&space;" title="P(K)=(K)((K)+1)/2 " />

So the statement must be true.


## Contrapositive

WIP


## Contradiction

WIP


## Final thoughts

WIP


## References 

- [Induction Models](https://arxiv.org/abs/2008.06410)
- [latex](https://latex.codecogs.com/)
