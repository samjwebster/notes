---
tags: 
    - lc-medium
    - lc-pinterest
    - lc-dp
    - lc-dfs
title: 91. Decode Ways
---

## Problem

You are given a string of integers that decode into a secret message accoring to lexicographical position (1 -> A, 2 -> B, ...). However, some substrings can be decoded in more than one way. For example, "26" can be decoded as 2 -> B and 6 -> F or 26 -> Z. Given a decoded string, return the number of ways that the string can be decoded. 

Examples:
```
s="12"
res = 2 # [1, 2]; [12]
```

```
s= "226"
res = 3 # [2, 2, 6]; [22, 6]; [2, 26]
```

## My solution

First attempt is recursion with memoization. Progress through the string, stepping forward by one position or two positions, if possible. Use DFS to proceed down and accumulate possible distinct paths. Minor memoization added - technically valid without it, but in this case the leetcode script had time expired, so I needed it. But at a given index, the possible solutions from that point is fixed, so when later branches see it you can reuse the count.

```
class Solution:
    def numDecodings(self, s: str) -> int:
        memo = dict()
        def recur(i):
            if i == len(s):
                return 1 # progressed the string exactly, landing right after it ends 
            elif s[i] == "0":
                return 0 # standalone or leading 0s aren't decodable

            if i in memo:
                return memo[i] 

            steps = recur(i+1) # always possible to take a single step
            if(i < len(s) - 1 and (s[i] == '1' or (s[i] == '2' and s[i+1] < '7'))):
                steps += recur(i+2) # possible to take a double step here

            memo[i] = steps
            return steps

        return 0 if len(s) == 0 else recur(0)
```

Translating a C++ solution is the first stage conversion to Dynamic Programming, using a dynamic programming array. Start by working backwards from the array. If you encounter a zero, you must jump over it by having it be part of a two-sized decode step. Otherwise, you can increment the current index with paths possible from the index + 1. Hard to explain when writing it down, but I think it makes sense in my head maybe? If we don't have zero, one step is valid, so we can accumulate all branches that stepping forward one step links us to.

If we can go two steps from this position (that is, i has enough room and `s[i:i+2]` is less than or equal to 26), add the possible decode paths from two positions forward to the current position. This is how the positions accrue as we travel down. 

Finally, if s wasn't empty, we can return the possible paths stored in the first element of the dp array, all paths reachable from the first position. 
```
class Solution:
    def numDecodings(self, s: str) -> int:

        dp = [0]*(1+len(s))
        dp[len(s)] = 1

        for i in range(len(s) - 1, -1, -1):
            if s[i] == '0':
                dp[i] = 0 # Must always be 'jumped over', aka the second element of a two-length decode
            else:
                dp[i] = dp[i + 1] # Otherwise, the current cell is decodable in one step, so it can be part of all paths at one step ahead
                if(i < len(s) - 1 and (s[i] == '1' or (s[i] == '2' and s[i+1] < '7'))): # If we can take two steps here...
                    dp[i] += dp[i + 2] # then it can be part of all the paths two steps ahead
        
        return 0 if len(s) == 0 else dp[0]
```

A final upgrade to the DP solution, taking it from linear to constant space. At any given position, there are only two checks we'd ever need to do: how many branches are possible one step and two steps forward. So, going down take the exact same checks. If `s[i] == '0'`, we can't take any steps from here. Otherwise, we can definitely take at least one step, so the current cell is initialized as carrying the one_step accessible branches. If we are valid for two steps, then the current cell can additionally carry the possible two step away branches. To continue down the string, update the two steps away branches with the one step count, and the one step away branches with the curr cell count. This is in accordance with the index decrementing. 

```
class Solution:
    def numDecodings(self, s: str) -> int:
        one_step, two_step = 1, 0

        for i in range(len(s) - 1, -1, -1):
            curr = 0 if s[i] == '0' else one_step # If we're not on a zero, we can always take one step
            if(i < len(s) -1 and (s[i] == '1' or (s[i] == '2' and s[i+1] < '7'))):
                curr += two_step # If two steps are possible, add those paths too
            # One step becomes two steps ago, current becomes one step ago
            two_step = one_step
            one_step = curr
        # We can always return on one step when there's no leading 0. 
        # A single nonzero digit is always decodable in one step, so it will contain all paths we can start from
        # Not always two steppable from the start (ie start on 3+)
        return 0 if len(s) == 0 else one_step
```