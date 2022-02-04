# Mathematical Proofs

## Introduction

For many of us, working in FAANG means 6 figures salary. Although this is correct, I would say working in FAANG for me, primarly means 
interracting with very smart and dedicated people. But there is a catch to this last statement. What does it mean to interact with very 
smart people ? What makes you think these people are smart ? What is your experience interacting with them ? you would ask...
Well, you guessed it, answering this question would probably take us a lot of time and involve a fair amount of subjectivity to it. Here, I want to focus
on an aspect that I believe is common amoung the very smart people I have met and interracted with. That is, things I heared, usually sound like proofs, yup... mathematical proofs. So in the following article, I discuss 3 powerful Mathematical proof techniques that I would consider helpful in FAANG environment, might it be
during interviews or in everyday life. For each technique, we will prove an example statement to get comfortable with the technique. Without further due, let's jump right in the techniques !


## Induction

Example :

<img src="https://latex.codecogs.com/svg.image?P(n)&space;=&space;\sum_{0}^{n}&space;i&space;=&space;n*(n&plus;1)/2&space;" title="P(n) = \sum_{0}^{n} i = n*(n+1)/2 " />

Proof : 

1. We start by proving a base case, P(1) for instance

<img src="https://latex.codecogs.com/svg.image?P(1)=0&plus;1&plus;2&plus;...&plus;1&space;=&space;1*(1&plus;1)/2" title="P(1)=0+1+2+...+1 = 1*(1+1)/2" />

2. We finish by proving that the statement holds for a number k + 1

<img src="https://latex.codecogs.com/svg.image?P(k&plus;1)=0&plus;1&plus;2&plus;...&plus;k&plus;1&space;=&space;k*(k&plus;1)/2&space;&plus;&space;(k&plus;1)" title="P(k+1)=0+1+2+...+k+1 = k*(k+1)/2 + (k+1)" />

Then

<img src="https://latex.codecogs.com/svg.image?P(k&plus;1)=0&plus;1&plus;2&plus;...&plus;k&plus;1&space;=&space;(k&plus;1)((k&plus;1)&plus;1)/2" title="P(k+1)=0+1+2+...+k+1 = (k+1)((k+1)+1)/2" />

We can see that if we replace k + 1 with K, 

<img src="https://latex.codecogs.com/svg.image?P(K)=0&plus;1&plus;2&plus;...&plus;K&space;=&space;(K)((K)&plus;1)/2" title="P(K)=0+1+2+...+K = (K)((K)+1)/2" />

Which shows that the statement holds for k + 1.
 

## Contrapositive

Example :

Prove that if <img src="https://latex.codecogs.com/svg.image?x^{2}" title="x^{2}" /> is even, then <img src="https://latex.codecogs.com/svg.image?x" title="x" /> is even

Proof : 

## Contradiction

Example :

<img src="https://latex.codecogs.com/svg.image?\sqrt{2}" title="\sqrt{2}" /> is irrational

Proof : 

## Final thoughts

WIP


## References 

-[]()
-[]()
-[]()
