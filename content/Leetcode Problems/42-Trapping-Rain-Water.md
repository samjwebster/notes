---
tags: 
    - lc-hard
    - lc-two-pointers
    - lc-dp
    - lc-revisit
title: 14. Longest Common Prefix
---

You are given an array `height`, where each index stores a height in a one-dimensional heightmap. The heightmap can trap water at positions which form sinks along the heightmap.

Example:

```
       X
   X~~~XX~X
_X~XX~XXXXXX

height = [0,1,0,2,1,0,1,3,2,1,2,1]
output = 6

6 units of water (~) are trapped by the heightmap.
```

## Solution

I had the right intuition on using maximums but could not reach a solution on my own. Translating a solution from leetcode:

```
class Solution:
    def trap(self, height: List[int]) -> int:
        
        total = 0
        l, r = 0, len(height) - 1
        max_l = max_r = 0

        while l <= r:
            if height[l] <= height[r]:
                if height[l] >= max_l:
                    max_l = height[l]
                else:
                    total += max_l - height[l]
                l += 1
            else:
                if height[r] >= max_r:
                    max_r = height[r]
                else:
                    total += max_r - height[r]
                r -= 1

        return total
```

The gist of the approach is to move iteratively inward and work in left-right phases based on whichever side has the new largest retaining wall. 

When left is less than (or equal) to right: (left case). Simply, right is the big retaining wall, so we're going to add whatever water is possible until we find new bigger left wall.
- We know that whatever water is trappable is bounded by the left max, and right is >= left max (because if it wasn't, we would be in the right case). 
- So, if our current left height is greater than or equal to our left max, we found a new left max and update it. Can't trap water, because whatever previous left max is less than the current position (it would overflow out of left bounds)
- If it is less than left max, we can trap water! Add the difference between left max and current left height to the result.

Else, when right is less than left: (right case). Simply, it's now left that is the big retaining wall, and we're going to see how much water we can add until we find a new right height (and max) that puts right back in charge
- We know that whatever water is trappable is bounded by the right max. And, left max is at least as large as right max (otherwise we'd be in the left case). 
- So, if the current right height is greater than or equal to the right max, update right max. Can't catch water here, or it would overflow out of the right bounds of the array.
- Otherwise, right max is greater and we know left is secure, so we can add the difference between right max and current right height to the result!

Another way to think about why this works is visually. The pools of water sorta form a pyramid of ascending max values. You move inward on whichever side has the lower values and a high opposite wall until it flips, then you move in from the other side, higher and higher.  You can visually see that inward pools get iteratively higher as a result of this pattern. 

This problem makes sense but is hard to wrap my mind around, because I get conceptually why it works when looking at the image, but I think the iterative conditions, knowing that the opposite side is at least as high as the current side when adding water, makes it hard to fully conceptualize the algorithm. Should revisit this!