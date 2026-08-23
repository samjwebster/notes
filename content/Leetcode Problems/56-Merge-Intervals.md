---
tags: 
    - lc-medium
    - lc-pinterest
    - lc-sort
title: 56. Merge Intervals
---

Given an array of intervals like `[start, end]`, return a new array with all overlapping intervals merged, ensuring that the new merged array fully covers intervals represented by the input.

Examples:
```
intervals = [[1, 3], [2, 5], [9, 10]]
result = [[1, 5], [9, 10]] # overlap between [1, 3] and [2, 5], so merged
```

## My Solution

Sort the array of intervals according to their starting element. This ensures that when we proceed through the array we can merge intervals in a single pass. 

Instantiate the new intervals with the first element. Begin iterating over the remaining intervals and comparing to the previous interval (`[-1]` in the new intervals array). If the current interval starts before or equal to when the previous interval starts, these are mergable. Update the previous interval to be the maximum of both intervals' ending values. If there's no overlap, just append the current interval, and subsequent checks now will compare to it. Once this is done, return the new merged intervals.

```
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if not intervals:
            return []

        intervals.sort(key = lambda pair: pair[0])

        new_intervals = [intervals[0]]
        for pair in intervals[1:]:
            if pair[0] <= new_intervals[-1][1]:
                new_intervals[-1][1] = max(new_intervals[-1][1], pair[1])
            else:
                new_intervals.append(pair)
        return new_intervals
```