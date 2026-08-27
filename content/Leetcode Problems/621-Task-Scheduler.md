---
tags: 
    - lc-medium
    - lc-pinterest
    - lc-heap
    - lc-revisit
title: 621. Task Scheduler
---

## Problem

You are CPU given an array `tasks` containing letter-denoted tasks to be completed, and an integer `n` which is the minimum interval gap between same-lettered tasks to be executed. So if there are two instances of task `A`, there must be at least `n` intervals executed between the two completions of `A`. Return the minimum number of intervals required to complete all tasks.

Examples:
```
tasks = [A,A,A,B,B,B]
n = 2
output = 8

A -> B -> idle -> A -> B -> idle -> A -> B
```

## Solution

Had to reference Leetcode solutions to figure it out. I was on the right track with getting frequencies and using a max heap to draw top tasks in each cycle, but got stuck thinking about cooldown - the solution uses a cool trick for that.

Get the frequencies of all distinct tasks and create a max heap (aka an inverted min heap, fitting to heapq's needs). 

While the heap exists, go in cycles of `1+n` intervals. This allows us to enter each loop with guarunteed nothing on cooldown, as the loop is the cooldown itself. 
I like to think of the `1+n` as `1`-> the top most-frequent task remaining and `n` -> the `n` next most frequent tasks to do while waiting for the `1` top task to cool down. 
So, 'execute' whatever `1+n` tasks are available in the heap by popping them and storing in a temporary array, and if you exhaust the heap in this phase break. Go through the tasks in the array; if they have more than one step remaining (with the one step being what you just executed), there's more to do for that task. Repush it to the heap with one less freq left to do (aka `freq+1`, since inverted min array).
At the end of the while loop, add `1+n` to the result if there's more loops to come (`heap` isn't empty). Otherwise, add the length of the `curr_phase`; we exhausted all remaining tasks in the middle of the cycle.


```
import heapq
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        # Get frequencies of all tasks
        freqs = dict()
        for t in tasks:
            freqs[t] = freqs.get(t, 0) + 1

        # Create a max heap (inverted min heap, so -freq) of frequencies
        heap = [-freq for freq in freqs.values()]
        heapq.heapify(heap)

        res = 0

        while heap:
            # Each loop, execute the next 1+n tasks
            # That is, the 1 current most frequent task, followed by the cooldown-period n tasks available
            curr_phase = []
            for _ in range(1+n):
                if not heap:
                    break
                curr_phase.append(heapq.heappop(heap))

            for freq in curr_phase:
                if freq < -1:
                    # More to do, so decrement the completed interval and readd to heap
                    heapq.heappush(heap, freq+1) # Add because we're max heap, aka inverted min heap
            
            # If heap is empty, we completed all tasks, so use len(curr_phase) in the case that the last loop didn't need 1+n steps to finish
            # Otherwise, we executed an entire 1+n phase and there's more to do.
            # Cannot default to len(curr_phase). Imagine a heap of [10] and n=2. You only do 1 task per 1+n intervals, but you still need to count those 1+n intervals not just the one task completed in those intervals. There was down intervals which are counted even though no task occured as it was all on cooldown
            res += len(curr_phase) if not heap else 1+n
        
        return res
```