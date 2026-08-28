---
tags:
    - nc-easy
    - nc-array
    - nc-hashmap
title: Two Sum
---

## Problem

Given an array of integers `nums` and an integer `target`, return the two indices `[i, j]` where `i<j` and `nums[i]+nums[j] == target`. 

## My solution

Keep a dict to track the index of the target minus the number at that index `diffs`. In a for loop, if the current number is in `diffs`, then we know we have the other element needed to sum to target, so return the pair of indices in order (whatever is in `diffs` necessarily came first). Otherwise, subtract the current number from target and store its index at that difference in `diffs`. I like to do it this way because you don't need to do math in the existence check; you can alternatively store a dictionary of what numbers we've seen where and check for `dict[target-n]`, essentially the flip of what I like to do, it would work identically.

```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        diffs = dict()
        for i, n in enumerate(nums):
            if n in diffs:
                return [diffs[n], i]
            else:
                diffs[target-n] = i
```