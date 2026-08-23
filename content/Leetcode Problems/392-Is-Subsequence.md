---
tags: 
    - lc-easy
    - lc-pinterest
    - lc-string
title: 392. Is Subsequence
---

## Problem

The problem asks, given input strings `s` and `t`, return `True` if `s` is a valid subsequence within `t`. Meaning, the characters of `s` appear in-order (doesn't have to be side-by-side) throughout `t`. Example:

```
s = "abc"
t = "a1b2c3"
output: True
```

```
s = "abcd"
t = "a1b2d3e4"
output: False # no 'c'
```

## My solution

Use a pointer tracking iterative position in `s`. Progress character-by-character through `t`. If the character matches what we need in `s`, increment the pointer. If the pointer index equals the length of `s`, we know we've progressed the entire subsequence and can return true. If we progress the entirety of `t` without this case occuring, we know we didn't finish the subsequence and can return False.

Two edge cases accounted for, had to add these as failed solutions appeared:
- If `s` is empty, it wants you to return True. I initially thought False, because `s` isn't really a sequence then so why is it a guarunteed subsequence, but whatever it's sorta just semantic
- If `s` is longer than `t`, we know `t` could never possibly fit a subsequence longer than the string itself, so immediately return False

```
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        if not s:
            return True

        if len(s) > len(t):
            return False
        
        ptr = 0

        for c in t:
            if c == s[ptr]:
                ptr += 1
                if ptr == len(s):
                    return True

        return False
```

