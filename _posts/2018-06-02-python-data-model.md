---
layout: post
title: "Names in Python"
date: 2018-06-02
categories: Python
---
# What's in a variable?
In most traditional languages, variable are named memory locations that can store values. A statement like ```variable = value``` stores the value into the memory location represented by variable. For example, given the following sequence:
{% highlight c %}
a = 1;
a = 2;
{% endhighlight %}
In a language like C, the first line puts the value ```1``` in a memory location referenced by the name ```a```, and the second line update the value to ```2``` in that same memory location. But this is not how things work in Python.

**In Python, a variable is a name used to refer to an object stored somewhere in memory.**  The same statement ```variable = value``` decides which object(value) the name is going to refer to. In the same sequence above, the first line makes ```a``` points to to the object with value ```1```, and the second line points the variable ```a``` to an object with value ```2```.

We can easily verify this by using the built-in functions ```id()``` and ```is```. Conceptually, ```id()``` returns the memory address of an object, and ```is``` check to see if two objects have the same id (aka in the same memory address, instead of check for equal value as ```==``` does).

{% highlight python %}
a = 1
print id(a)
#=> 140441894830760
a = 2
print id(a)
#-> 140441894830736
{% endhighlight %}

We can see that as we rebinds ```a```, its id changes, reflecting the fact that ```a``` now points to a different memory location. To further illustrate the point:

{% highlight python %}
a = 1
b = a
print id(a)
#=> 140441894830760
print id(b)
#=> 140441894830760
print a is b
True
{% endhighlight %}

As we can see, ```b=a``` indeed makes ```b``` point to the same object as ```a``` does, and they now have the same id. Knowing this can help us understand why the following code behaves as it does:

{% highlight python %}
a = [1, 2, 3]
b = a
b[0] = 0
print a
#=> [0, 2, 3]
{% endhighlight %}

Here is a nice practical summary:
{% highlight python %}
B = A
if A is immutable:    # int, string, tuple, ...
    if B changes:  A does not change
if A is mutable:      # list, dict, ...
    if B is modified in place:  A also changes
    elif B is rebinded to something else:  A does not change
{% endhighlight %}


# Names as function arguments
A related question is whether function arguments are passed by value or by reference in Python. Well, they are passed by value, but since the names are themselves just references, it is a little more complicated.

To repeat: **the parameter passed is actually a reference to an object, but the reference is passed by value.**

So, 
1. If you pass a mutable object into a method, the method gets a referene to that same object, and you can mutate it, but if you rebind the reference in the method, the outer scope will know nothing about it . After you are done, the outer reference will still point at the original object. 

2. If you pass an immutable object to a method, you still can't rebind the outer reference, and you can't even mutate the object. 
 
Let's see some examples.

{% highlight python lineno %}
def update(x):
    x.append(1)

def rebind(x):
    x = [1]

x = [0]
update(x)
print x
#=> [0, 1]
x = [0]
rebind(x)
print x
#=> [0]
{% endhighlight %}

# A bit about application
What if we insist on changing the value of an immutable object passed to a function? Surely this could be useful in some circumstances, right? Well, there are two ways to work around that in Python. The first one is to simply ```return``` a value from within the function and rebind it outside. To illustrate:

{% highlight python %}
def change(immutable):
    immutable = 'abc'
    return immutable

str = 'a'
str = change(str)   
{% endhighlight %} 

This is IMO the better and more readable way. The other method is to transform an immutable object to a mutable one before we make changes to it. 
{% highlight python %}
def change(something):
    something[0] = 'abc'
    
str = 'a'    
str_dummy = [str]
change(str_dummy)
do_something_with(str_dummy[0])
{% endhighlight %}

As you can see, this is pretty ugly. It's only included here for the purpose of completeness, please don't use it in your code.


Disclaimer:
> This post is migrated from my github account, where I used to keep study notes. The content is taken from various online resources, mostly stackoverflow. I don't claim to have come up with it myself.

