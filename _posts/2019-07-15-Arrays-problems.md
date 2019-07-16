
## Arrays Problems [ WORK IN PROGRESS ]

- Two Sum - https://leetcode.com/problems/two-sum/
  
    ```bash
    Given an array of integers, return indices of the two numbers such that they add up to a specific target.
    You may assume that each input would have exactly one solution, and you may not use the same element twice.
  
    Example:
    Given nums = [2, 7, 11, 15], target = 9,
    Because nums[0] + nums[1] = 2 + 7 = 9,
    return [0, 1].
  ```
  
  #### The brute force solution
  
  It's very easy to see that to find a complementary integer that sums up to the target, you need 2 loops.
  The first loop will take each item of the list one by one, and check for each elements of the second list
  to see if a different element of the list is the complementary element.
  In the example :
  we take 2 at position 0 from the first loop, then we take the second loop, 2 is at the same position as the first loop 
  index, so we take 7 at position 1. we can see that 2 + 7 - target == 0. so we return immediately the couple {0, 1}
  Complexity is then O(n^2)
  
  #### The efficient solution
  
  We can use a map to efficiently search for the complement of a number with regards to the given target number.
  In the example :
  we take 2, we ask the question : Is the complement of 2 which is 7 inside the map ? The answer is no, then we add the 
  complement to the map :
  map contains : {2, 0}
  we take 7, we ask the question : Is the complement of 7 which is 2 inside the map ? The answer is yes, then we 
  take the index of 2 which is 0 and the index of the current element which is 1 and form the couple  {0, 1}.
  Because the map used is an unordered_map, the access to an element is O(1).
  The amortized complexity will be O(1)
  
