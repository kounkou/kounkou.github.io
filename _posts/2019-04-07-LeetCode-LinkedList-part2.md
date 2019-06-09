---
layout: post
title: "LeetCode::LinkedList(Part2)-Modifying the List"
date: 2019-04-23
mathjax: true
categories: Algorithm
---
## Introduction
------
Most linked list algorithms are conceptually quite straightforward. The main difficult lies in that fact that when modifying a node in the list, often times you also **need to have reference to the node just before it or after it**. Take for example a simple case of deleting the node ``B`` from list ``A->B->C``. After you identify where ``B`` is, you still need to keep a reference of ``A`` in order to write ``A->next = B->next``. 

This brings code complexity, as you need to keep track of more variables and treat special cases carefully. The head node obviously has no previous node and the ``NULL`` node has no node after it. I usually approach a problem in the following steps:

1. **Design the inside of ``while`` loop**

    In this step we ignore edge cases and focus on how to modify the body of the list. A ``cur`` node, initialized as the ``head`` node before we enter the loop, will always be the current node that we are working on.

2. **Decide the break point for the loop**

    This will usually be when our ``cur`` node reaches ``NULL``, but exceptions exsist. For example, if we referenced``cur->next->value`` in the loop, since a ``NULL`` node has no value filed, we must break out of the loop when ``cur->next`` is ``NULL``.

3. **Handle edge cases**

    The ``head`` node usually needs to be handled seperately. 

# 83. Remove Duplicates from Sorted List
Given a sorted linked list, delete all duplicates such that each element appear only once.

Example:

Input: ``1->1->2->3->3``

Output: ``1->2->3``

# Solution 1 (Iterative)
{% highlight c %}
ListNode* deleteDuplicates(ListNode* head) {
    ListNode *cur = head;
    while (cur && cur->next) {
        if (cur->val == cur->next->val)
            cur->next = cur->next->next;

        cur = cur->next;
    }
    return head;
}
{% endhighlight %}
# Solution 2 (Recursive)
{% highlight c %}
struct ListNode* deleteDuplicates(struct ListNode* head) {
    if (head == NULL || head->next == NULL)
        return head;
    
    struct ListNode* rest = deleteDuplicates(head->next);
    if (head->val == rest->val){
        // return rest;
        head->next = rest->next;
        return head;
    }
    else
        return head;
}
{% endhighlight %}

## When you need the node **Before** ``cur``
------

# 82. Remove Duplicates from Sorted List II
Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

Example:

Input: ``1->2->3->3->4->4->5``

Output: ``1->2->5`` 

# Solution 1 (Iterative)
{% highlight c %}
struct ListNode* deleteDuplicates(struct ListNode* head){
    struct ListNode dummy = {-1, head};
    struct ListNode* pre = &dummy;
    struct ListNode* cur = head;
    
    while(cur && cur->next){
        if (cur->val == cur->next->val){
            while(cur->next && cur->next->val == cur->val){
                cur = cur->next;
            }
            pre->next = cur->next;
        }
        else{
            pre = pre->next;
        }
        cur = cur->next;
    }
    return dummy.next;
}
{% endhighlight %}

# 203. Remove Linked List Elements
Remove all elements from a linked list of integers that have value ``val``.

Example:

Input: ``1->2->6->3->4->5->6``, val = 6

Output: ``1->2->3->4->5``

# Solution 1
{% highlight c %}
struct ListNode* removeElements(struct ListNode* head, int val) {
    struct ListNode dummy = {-1, head};
    struct ListNode* pre = &dummy;
    struct ListNode* cur = head;
    
    while(cur){
        if (cur->val == val){
            pre->next = cur->next;
        }
        else{
            pre = pre->next;
        }
        cur = cur->next;
    }
    return dummy.next;
}
{% endhighlight %}

## When you need the node **After** ``cur``
------
haha

# 206. Reverse Linked List
Reverse a singly linked list.

Example:

Input:  ``1->2->3->4->5->NULL``

Output: ``5->4->3->2->1->NULL``

 
# Solution 1 (Iterative)
When you need the node after temp


{% highlight C %}
struct ListNode* reverseList(struct ListNode* head) {
    struct ListNode* prev = NULL;
    struct ListNode* cur = head;
   
    while (cur){
        struct ListNode* temp = cur->next;
        cur->next = prev;

        prev = cur;
        cur = temp;
    }
    return prev;
}
{% endhighlight %}
As you can see, although we have to change the loop condition accordingly, the logic works exactly the same. 

The Python version of the same code is quite a bit shorter:

{% highlight python %}
def reverseList(head):
    pre, cur = None, head
    while cur:
        cur.next, pre, cur = pre, cur, cur.next
    return pre
{% endhighlight %}
This is the most popular solution on LeetCode, is 1 line shorter compared to using ``cur`` and ``nex``, but perfonally I don't like it much. Mainly because such a use of ``pre`` is the most typical and may confuse people. 

# Solution 2 (Recursive)
{% highlight C %}
struct ListNode* reverseList(struct ListNode* head) {
    if (head == NULL || head->next == NULL){
        return head;
    }
    
    struct ListNode* tail = reverseList(head->next);
    head->next->next = head;
    head->next = NULL;
    return tail;
}
{% endhighlight %}

After the first recursive call, we have:
{% highlight python %}
1->2<-3<-4<-5
   ↓
  NULL 
{% endhighlight %}
while ``tail`` is at 5 and ``head`` still at 1. We fix the ``1->2`` link with ``(head->next)->next = head`` and add ``1->NULL``.

# 24. Swap Nodes in Pairs
Given a linked list, swap every two adjacent nodes and return its head.

You may not modify the values in the list's nodes, only nodes itself may be changed.

Example:

Input: ``1->2->3->4``

Output: ``2->1->4->3``

# Solution 1
{% highlight c %}
struct ListNode* swapPairs(struct ListNode* head){
    struct ListNode dummy = {-1, head};
    struct ListNode* pre = &dummy;
    struct ListNode* cur = head;
    
    while(cur && cur->next){
        struct ListNode* nex = cur->next;
        cur->next = nex->next;
        pre->next = nex;
        nex->next = cur;
        
        pre = pre->next->next;
        cur = pre->next;
        
    }
    return dummy.next;
}
{% endhighlight %}


