---
tags:
    nc-easy
    nc-dp
    nc-revisit
title: Climbing Stairs
---

## Problem

You are given an input `n`, which represents the numbers of stairs in a staircase. You can traverse the staircase in one or two steps at a time. Return the total number of distinct ways you can climb the staircase.

## Solution

Consider you are at step 3. How can you get here? Well, you can do it from one step ago or two steps ago, either via a single step from one step ago or a double step from two steps ago. This is the intuition behind the DP solution - the `i`th step's number of distinct paths is the sum of the `i-1`'s and `i-2`'s distinct paths. 

```
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 1:
            return 1
        dp = [0] * n
        dp[0] = 1
        dp[1] = 2
    
        for i in range(2, n):
            dp[i] = dp[i-1] + dp[i-2]
        return dp[-1]
```

You can space optimize this method, because at any given step you only need to know the distinct ways to get to one step ago and two steps ago.

```
class Solution:
    def climbStairs(self, n: int) -> int:
        one = 1
        two = 1

        for i in range(n-1):
            tmp = one
            one = one + two
            two = tmp
        
        return one
```