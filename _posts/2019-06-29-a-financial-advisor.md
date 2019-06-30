# A financial advisor for your investments


## Introduction

Let me make it straight and ask the big question now : is there any way to find the best investment strategy over the course
of a certain number of months ?
That question might sound innocent and pristine at first glance, but give me a chance and keep reading,
it will be worth it.
I am in Vancouver region, and bank's compition is fierce here. With inflation going up and scaring us all, some banks
are really working hard and providing amazing interest rates for saving and retirement accounts that are so interesting that
it's almost impossible to resist,... If you have no idea what I am talking right now, I guess you're probably very rich, or
with a higher probability less than average and struggling hard with your finance.
Maybe you want to save, or maybe you just want to invest, and make some money out of your money and you just realized your
0.55% or that 1.5% savings account arenot good enough to keep up with the cost of living ?
In this article I am providing outputs from a project I am working on, to maximize your return on investment for specific banks located in Canada. But the code source can be adapted with minimum effort to any bank.


## Some scary graphs

Let me start with 2 graphs. These 2 graphs represent all possible investments actions that can be taken
by someone for a period of 12 months. If these 2 graphs scared you, you can now understand the reason why I started this 
post. The complexity of the constraints allow us to represent the possible investments during that period
with a set of edges, connected with nodes representing months.

- Interest constraints for Bank A
<img width="647" alt="Screen Shot 2019-06-29 at 10 06 51 PM" src="https://user-images.githubusercontent.com/2589171/60392666-996b8380-9abc-11e9-9cb6-ffc574f7ae11.png">

- Interest constraints for Bank B
<img width="644" alt="Screen Shot 2019-06-29 at 10 27 07 PM" src="https://user-images.githubusercontent.com/2589171/60392690-30384000-9abd-11e9-9da1-7be45b395f6f.png">


## The Question to ask

If you haven't been scared by those graphs, you might ask yourself. Why do we need those complex graphs ?
The answer is pretty simple, we want to find the best set of edges from the beginning to the end of the graph. why ?
Well, that set of best paths will yield the best combination of interest rates to make the maximum profit.

## Closing thought

To close this post, I wanted to performed some tests and show some example of simulation :

- Bank A

```bash
0 ->  2                  7000
2 ->  5 interest : 0.5 : 7035
5 -> 11 interest : 2   : 7175.7

You will make : $7175.7 from a $7000 investment 
```

- Bank B

If we were to run an algorithm, one of the best paths throught the graph A would yield the following result :

```bash
0 ->  2                    7000.00
2 ->  5 interest : 0.756 : 7052.92
5 ->  8 interest : 0.756 : 7106.24
8 -> 11 interest : 0.756 : 7159.96

You will make : $7159.96 from a $7000 investment 
```


It seems pretty obvious, that you should probably go to bank A !
