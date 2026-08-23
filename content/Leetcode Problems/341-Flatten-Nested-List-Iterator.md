---
tags: 
    - lc-medium
    - lc-pinterest
    - lc-queue
    - lc-string
title: 341. Flatten Nested List Iterator
---

## Problem

You are given a list of NestedInteger objects, which are themselves a list of either Integers or more NestedInteger lists. You can learn if a particular entry is an integer via `li.isInteger()`, retrieve the value with `li.getInteger()`, and if not an integer get the list as `li.getList()`. 

Write a data structure called NestedIterator for flattening these nested integer objects. It should be initialized with a nested integer, have a `next()` function that returns the next element in the flattened traversal, and a `hasNext()` function for returning whether the traversal has been completed.

## My solution

I initially started with a nested approach which was working but I couldn't quite get figured out. I opted instead for a flattened parsing approach that dissects the NestedInteger object into a literal flattened list. I then proceed easily through this as a queue, popping the left element within `next()` and returning whether length is greater than 0 in `hasNext()`. This solution feels clean but a bit less elegant/technical than a recursive approach

```
class NestedIterator:
    def __init__(self, nestedList: [NestedInteger]):
        def flatten(nested):
            result = []
            for elem in nested:
                if elem.isInteger():
                    result.append(elem.getInteger())
                else:
                    result.extend(flatten(elem.getList()))
            return result

        self.q = deque(flatten(nestedList))
    
    def next(self) -> int:
        return self.q.popleft()
    
    def hasNext(self) -> bool:
        return len(self.q) > 0
```