---
tags:
    - nc-easy
    - nc-string
---

## Problem

Given an array, return True if any value appears more than once (has a duplicate)

Examples:

```
nums = [1,2,3]
res = False
```

```
nums = [1, 3, 2, 1]
res = True # 1 appears twice
```

## My solution

Two simple ways to achieve this:

Converting to set removes duplicates. So, if we have a duplicate in nums, the length of the set will be less than the length of nums, as the duplicate was collapsed.

```
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        return len(set(nums)) < len(nums)
```

Iterate over the list and maintain a set `seen` of all numbers encountered. If we encounter a number in `seen`, then that's a duplicate, return True. Otherwise, we go through the whole list and don't find one, return False.

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