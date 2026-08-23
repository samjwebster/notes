---
tags: 
    - lc-easy
    - lc-pinterest
    - lc-cycle
    - lc-two-pointers
title: 202. Happy Number
---

## Problem 

The problem asks you to determine whether a number is 'happy', meaning that if you repeatedly square the sums of all number digits it will eventually equal 1.

Example:
```
n = 19
1^2 + 9^2 = 82
8^2 + 2^2 = 68
6^2 + 8^2 = 100
1^2 + 0^2 + 0^2 = 1 (happy :D )
```

```
n = 2 (not happy ;-; )
```

## My solution:
### Solution 1: Caching

Repeatedly perform the sum of squared digits calculation, storing a cache of results. If we see something from the cache, then a cycle has occured. If we see 1, then we're happy.

```
class Solution:
    def isHappy(self, n: int) -> bool:
        cache = set()
        while n != 1:
            if n in cache:
                return False
            
            cache.add(n)
            n = sum([int(i)**2 for i in str(n)])

        return True
```

### Solution 2:

Use a two racing pointers solution. One is slow and does the calculation once per loop, and the other is fast and does it twice. There are two situations where these can be equal: either we have found a cycle, which they will eventually both be trapped and overlap during, or we've reached one. In the happy calculation, 1 will return itself, so the fast pointer will reach 1 before the slow pointer, meaning this loop will certainly eventually end on either a cycle or a 1. Therefore, after breaking, whether or not slow == 1 answers if we exited due to being happy (True) or being in a cycle (False).

```
class Solution:
    def doCalc(self, n: int): 
        return sum([int(i)**2 for i in str(n)])

    def isHappy(self, n: int) -> bool:   
        slow = n
        fast = n

        while True:
            slow = self.doCalc(slow)
            fast = self.doCalc(self.doCalc(fast))

           if slow == fast:
                break
        
        return slow == 1
```