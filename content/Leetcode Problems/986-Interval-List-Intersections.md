---
tags: 
    - lc-medium
    - lc-pinterest
    - lc-two-pointers
    - lc-sliding-window
title: 986. Interval List Intersections
---

[Leetcode](https://leetcode.com/problems/interval-list-intersections/)

## Problem

You are given two lists of sorted distinct intervals. Return a list of all times where the intervals in each list intersect.

Example:
```
firstList = [[0,2],[5,10]]
secondList = [[1,5],[8,12]]

res = [[1,2],[5,5],[8,10]]
```

## My Solution

Had a bit hard time with this one, referenced a solution. 

Basically, want to work iteratively with both lists and increment pointers one at a time, depending on which interval is 'exhausted'.
So, for the current pair of intervals, check for the criss-cross pattern based on their starting and ending points. If both intervals start before the other ends, then there is an overlap. The interval itself is the from the maximum of the interval starts and to the minimum of the interval ends. Increment pointer based on whichever interval ends before the other (`i_end <= j_end`, so if i ends before j ends increment i, otherwise increment j because it ends first).

```
class Solution:
    def intervalIntersection(self, firstList: List[List[int]], secondList: List[List[int]]) -> List[List[int]]:
        i = 0
        j = 0
        intersections = []
        while i < len(firstList) and j < len(secondList):
            i_start, i_end = firstList[i]
            j_start, j_end = secondList[j]

            # Intervals have a criss-cross shape
            # Both of these will be true. 
            if i_start <= j_end and j_start <= i_end:
                intersections.append([max(i_start, j_start), min(i_end, j_end)])
            
            if i_end <= j_end:
                i += 1
            else:
                j += 1
            
        return intersections
```