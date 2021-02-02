
# [DP] maximale non-consecutive sum

## Introduction

Dynamic programming is both a mathematical optimization method and a computer programming method. The method was developed by Richard Bellman in the 1950s and has found applications in numerous fields, from aerospace engineering to economics. It is to me the most crucial technic to have under your belt when you deal with very complex problems.

## Dynamic Programming

In this article we are trying to solve the max non-consecutive sum problem.

```go
int MCS(vector<int>& nums, i int) int {
   if (i >= len(nums)) {
      return 0
   }
   
   return max(nums[i] + MCS(nums, i+2),
                        MCS(nums, i+1))
}
```

## Final thoughts

<img src="https://latex.codecogs.com/svg.latex?\Large&space;\Theta(\log_{2}(n))" title="logarithm" />


## References 
