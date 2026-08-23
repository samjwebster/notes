---
tags: 
    - lc-medium
    - lc-pinterest
    - lc-dp
    - lc-string
title: 718. Maximum Length of Repeated Subarray
---

## Problem

You are given two integer arrays `nums1` and `nums2`. Find the maximum length of subarray repeated between these two arrays

Examples:
```
nums1 = [1, 3, 2, 5, 7]
nums2 = [6, 9, 2, 5, 7, 8]
res = 3 # 2, 5, 7 is the longest integer subarray
```

## My solution
First attempt, this works but its comically poor performance. Though my intuition was correct! Essentially, slide around `nums1` and compare it to nums2, finding the longest sequence of equal numbers in each repositioned version of the `nums1` array.

```
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        # For now, let's assume they're equal in length
        res = 0
        for i in range(-len(nums1) + 1, len(nums1) + 2):
            cycled_nums1 = [nums1[idx] if (idx >= 0 and idx < len(nums1)) else -1 for idx in range(i, i + len(nums1))]
            mask = [x == y for x, y in zip(cycled_nums1, nums2)]
            curr_res = 0
            # print(cycled_nums1, mask)
            for m in mask:
                if m == True:
                    curr_res += 1
                    res = max(res, curr_res)
                else:
                    curr_res = 0
            # nums1 = nums1[1:] + [nums1[0]] # cycle first to back
        
        return res
```

Second attempt after watching youtube video, which points out that this is extremely similar to the problem of finding the longest shared substring. It's a dynamic programming problem!

Instead of sliding around the entire integer array in order to see where and for how long the match is, slide each character around one at a time, using a DP memo/cache to track. For each integer in `nums1` at each position in `nums2`, is there a match? If there is, add 1 plus any accrued match from earlier in the string, according to `memo[i-1][j-1]`. Since we are going incrementally through nums1, we aren't going to miss anything. The longest subarray will stack this way, and we can maximize `res` as we find matches, and finally return it.

```
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        memo = [[0 for _ in range(len(nums2))] for _ in range(len(nums1))]
        res = 0

        for i in range(len(nums1)):
            for j in range(len(nums2)):
                if nums1[i] == nums2[j]:
                    memo[i][j] = 1 + (memo[i-1][j-1] if i > 0 and j > 0 else 0)
                    res = max(res, memo[i][j])
        return res
```

Third attempt is the same principle of the 2D dynamic programming solution, but in one dimension. This uses two arrays tracking only the previous and current integer from `nums1` as we iterate through, sliding around each integer looking for matches. Remember that the longest subarrays accumulate as we iterate over `nums1`. That is, if there are active subarrays, the single `prev_row` memo will have all of the data from the entire history of the string. After comparing against it, looking at `prev_row[i-1]` at matches to accumulate, and maximizing `res`, we update `prev_row` with `curr_row`. Return `res` with the solution. 

```
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        prev_row = [0] * len(nums2)
        res = 0

        for n in nums1:
            curr_row = [0] * len(nums2)
            for i in range(len(nums2)):
                if n == nums2[i]:
                    curr_row[i] = 1 + (prev_row[i-1] if i > 0 else 0)
                    res = max(curr_row[i], res)
            prev_row = curr_row
        return res

```.