---
layout: post
title: "LeetCode Series: Array"
date: 2018-10-18
mathjax: true
categories: Algorithm
---
## 1. 2Sum
# Description
Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly one** solution, and you may not use the same element twice.

# Example
Given nums = [2, 7, 11, 15], target = 9,

return [0, 1].

# Thghouts
Note that we are asked to return the indices of the numbers, not the values. So we can't just throw everything in a set and use membership checking. A more fitting data structure to use is dictionary, with numbers as dictionary keys and indices as dictionary values.

The most intuitive way to do this is 2-pass: with the first pass we build the dictionary as discussed above and with the second pass we check if the desired value is in the dictionary. However, there is also a 1-pass solution to the problem:

# Solution
{% highlight python %}
def TwoSum(nums, target):
        d = {}
        for (index, val) in enumerate(nums):
            want = target - val
            if want in d:
                return [d[want],index]
            else:
                d[val] = index    
{% endhighlight %}

Runtime: O(n)

Space: O(n)

## 15. 3Sum
# Description
Given an array of n integers, are there elements a, b, c in it such that a + b + c = 0? Find **all unique** triplets in the array which gives the sum of zero.

Note: The solution set must not contain duplicate triplets.

# Example
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]

# Thoughts
**1st Try**

We can modify the solution from the TwoSum problem to return all unique pairs of values that adds up to a certain value. We can then loop through the array and for each value we find a pair of numbers that adds up to the negation of it. 
{% highlight python %}
N, sols = len(nums), []

for i in range(N):
    two_sums = TwoSum(nums[i:], traget = -num):     
    for [num1, num2] in two_sums:
        sol = [num, num1, num2]
        if sol not in sols:
            sols.append(sol)
return sols
{% endhighlight %}

However, there are some challenging aspects about this approach. **First**, while looping, it is difficult/costly to exclude the current element from the array from which to find the two sums. (Think about an example of ``[-1, 2]``, it should return no answer instead of ``[-1, -1, 2]``). **Secondly**, it is difficult to remove duplicates from the solution set. Since the answer we want is List[List[int]], and a list is **unhashable**(cannot be used as a key in a dictionary), the easiest way of ``list(set(solutions))`` does not work here. If we go through each solution and check them indivisually, since there could be ${n \choose 3}$ solutions(think of an array of entirely 0's), we failed to reach a sub $O(n^3)$ algorithm.

**2rd Try**

After some failed experiments, I found a solution posted by user "caikehe" on Leetcode that handles both problems elegantly. The idea is to sort the numbers first, then for each number in the array, use two pointers on the remaining part to efficiently locate a solution and avoid duplicates. 

# Solution
{% highlight python %}
def ThreeSum(nums):
    nums.sort()
    N, sols = len(nums), []
    
    for i in range(N-2):
        if nums[i] > 0: break   # cannot be 3 positives 
        if i > 0 and nums[i] == nums[i-1]:continue

        start, end = i+1, N-1
        while start < end:
            if (nums[start] + nums[end]) < -nums[i]:
                start += 1
            elif (nums[start] + nums[end]) > -nums[i]:
                end -= 1
            elif (nums[start] + nums[end]) == -nums[i]:
                sol = [nums[i], nums[start], nums[end]]
                while start < end and nums[start] == nums[start+1]:
                    start += 1
                while start < end and nums[end] == nums[end-1]:
                    end -= 1
                sols.append(sol)
                start += 1
                end -= 1
  
    return sols
{% endhighlight %}

Notes:

`` if i>0 and nums[i] == nums[i-1]: continue`` To remove duplicate nums[i]

`` while start < end and nums[start] == nums[start+1]: start += 1`` To remove duplicate nums[start]

`` while start < end and nums[end] == nums[end-1]: end -= 1`` To remove duplicate nums[end]

Runtime: $O(n^2)$

Space: $O(n^2)$

# Important Take away
Sorting(having order) makes it really easy to avoid duplicate solutions to a problem.

## 16. 3Sum Closest
# Description
Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

# Example
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2). So return 2.

# Thoughts
Very similar idea. We loop through the sorted array, and for each element we use two pointers on the remaining array to get 3sums. We compare it with a "best_so_far" solution kept outside of the loop, and update it.

The interesting part is actually to come up with "bad" initial values. 

# Solution
{% highlight python %}
def ThreeSumClosest(nums, target):
    nums.sort()
    N, sols = len(nums), []

    # initial dummy values
    best_diff_so_far = abs(nums[0] + nums[1] + nums[2] - target) + 1    
    ans = nums[0] + nums[1] + nums[2]
    
    for i in range(N):
        if i > 0 and nums[i] == nums[i-1]:  continue    # skip if we have seen this before
        start, end = i+1, N-1
        
        while start < end:
            diff = abs(nums[start] + nums[end] + nums[i] - target)
            if diff < best_diff_so_far:
                best_diff_so_far = diff
                ans = nums[start] + nums[end] + nums[i]
                
            if nums[start] + nums[end] + nums[i] < target:
                start += 1
            elif nums[start] + nums[end] + nums[i] > target:
                end -= 1
            elif nums[start] + nums[end] + nums[i] == target:
               return target  
    return ans
{% endhighlight %}

# Notes
There is plenty of room for optimization. Skipped for readability. 

Runtime: $O(n^2)$

Space: $O(1)$

# Important Take away
Sorting(having order) is great when we are shooting for a range or a "closest" answer to a problem, instead of the exact solution. Hashtables are not suited for this.



