# Mathematical Proofs

<img width="1000" height="450" alt="Screenshot 2023-09-18 at 11 33 39 PM" src="https://github.com/kounkou/kounkou.github.io/assets/2589171/fb711bbc-17f8-4abf-b442-21d2f0ff6949">
Hinda

## Introduction

For many of us, working in FAANG means six-figure salary. Although this is correct, I would say working in FAANG for me, primarly means 
interracting with very smart and dedicated people. But there is a catch to this last statement. What does it mean to interact with very 
smart people? What makes you think these people are smart? What is your experience interacting with them? you would ask...
Well, you guessed it, answering this question would probably take us a lot of time and involve a fair amount of subjectivity to it. Here, I want to focus
on an aspect that I believe is common among the very smart people I have met and interracted with. That is, things I heared, usually sound like proofs, yup... mathematical proofs. So in the following article, I discuss 3 powerful Mathematical proof techniques that I would consider helpful in FAANG environment, might it be
during interviews or in everyday life. For each techniques, we will prove an example statement to get comfortable with the technique. Without further due, let's jump right in the techniques!


## Induction

Example:

<img src="https://latex.codecogs.com/svg.image?P(n)&space;=&space;\sum_{0}^{n}&space;i&space;=&space;n*(n&plus;1)/2&space;" title="P(n) = \sum_{0}^{n} i = n*(n+1)/2 " />

Proof: 

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

Example:

Prove that if <img src="https://latex.codecogs.com/svg.image?x^{2}" title="x^{2}" /> is even, then <img src="https://latex.codecogs.com/svg.image?x" title="x" /> is even

Proof: 

We start by finding the contrapositive statement.

if <img src="https://latex.codecogs.com/svg.image?x" title="x" /> is odd then <img src="https://latex.codecogs.com/svg.image?x^{2}" title="x^{2}" /> is odd

We can easily prove that the product of 2 odd number is odd: 

<img src="https://latex.codecogs.com/svg.image?(2*n&plus;1)*(2*n&plus;1)=4n^{2}&plus;4n&plus;1" title="(2*n+1)*(2*n+1)=4n^{2}+4n+1" />

which can be written as 

<img src="https://latex.codecogs.com/svg.image?(2*n&plus;1)*(2*n&plus;1)=2(2n^{2}&plus;2n)&plus;1" title="(2*n+1)*(2*n+1)=2(2n^{2}+2n)+1" />

Then if <img src="https://latex.codecogs.com/svg.image?x^{2}" title="x^{2}" /> is even, then <img src="https://latex.codecogs.com/svg.image?x" title="x" /> is even



## Contradiction

Example:

<img src="https://latex.codecogs.com/svg.image?\sqrt{2}" title="\sqrt{2}" /> is irrational

Proof: 

Proof by contradiction is probably the most powerful technique! But it can also hard for your neurones.

<img src="https://latex.codecogs.com/svg.image?\sqrt{2}" title="\sqrt{2}" /> is irrational, then it can be written as <img src="https://latex.codecogs.com/svg.image?\frac{a}{b}" title="\frac{a}{b}" />

where a or b is odd. However, if <img src="https://latex.codecogs.com/svg.image?\sqrt{2}" title="\sqrt{2}" /> is <img src="https://latex.codecogs.com/svg.image?\frac{a}{b}" title="\frac{a}{b}" /> then <img src="https://latex.codecogs.com/svg.image?a^{2}&space;=&space;2*b^{2}&space;&space;" title="a^{2} = 2*b^{2} " /> which means that a must be even, but also that b must be odd!

But if a is even, <img src="https://latex.codecogs.com/svg.image?a^{2}" title="a^{2}" /> is a multiple of 4, which means that <img src="https://latex.codecogs.com/svg.image?2b^{2}" title="2b^{2}" /> is a multiple of 4, meaning that b is even!

There is a contradiction for b that cannot be at the same time odd and even. Then the original statement 

<img src="https://latex.codecogs.com/svg.image?\sqrt{2}" title="\sqrt{2}" /> is irrational is false.


## Final thoughts

In this article we have explored 3 powerful proof techniques that are, Induction, Contrapositive and Contradiction. I have chosen to use Mathematical logical to showcase each of them, but the real purpose for you would now to use them in real life. If you have come so far, congratulations and let me know if any comments.

## References 

- [Induction](https://en.wikipedia.org/wiki/Mathematical_induction)
- [Contrapositive](https://en.wikipedia.org/wiki/Contraposition)
- [Contradiction](https://en.wikipedia.org/wiki/Proof_by_contradiction)
