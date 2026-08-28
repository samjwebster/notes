---
tags:
    - nc-easy
    - nc-string
title: Valid Anagram
---

## Problem

Given two strings `s` and `t`, return true if the strings are valid anagrams of each other, otherwise return False.

## My solution

If the lengths of `s` and `t` differ, return False as anagram is impossible. Otherwise, return True if the strings are equal when sorted.

```
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        seen = set()
        for n in nums:
            if n in seen:
                return True
            else:
                seen.add(n)
        return False
```