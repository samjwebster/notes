---
tags: 
    - lc-easy
    - lc-pinterest
    - lc-string
title: 14. Longest Common Prefix
---

## Problem

This question asks, suppose you have an input array of strings `strs`. What is the longest common prefix shared by these strings? That is, the longest string sequence appearing at the beginning of all strings

Example:
```
strs = ["flower", "flow", "float"]
prefix = "fl"
```

```
strs = ["hello", "goodbye"]
prefix = "" # no shared prefix, they begin different characters
```

## My solution

To safely code an algorithm with the expectation of multiple valid strings, we first get two critical edge cases out of the way:
- if the length of `strs == 0`, there are no strings to have a prefix to begin with, so immediately return `""`. I think leetcode says this isn't possible input, but i have it there anyways
- if there's only one string in strs, we can immediately return it, since it doesn't have anything to compare to for a shared common prefix. Therefore, it is the prefix itself

With those edge cases out of the way, we certainly have more than one string to compare. The longest possible prefix would be equal in length to the shortest string in `strs`. So, iterating pointer `i` from 0 to that minimum string length aka maximum prefix length. Since we're looking for commonality, we can take the characters sequentially from `strs[0]` to see if they are shared among `strs[1:]`. If we find a character in one of the other strings at `i` that does not equal what we found at index `i` for `strs[0]`, we can immediately return whatever the prefix currently is, as we just confirmed we cannot no longer add any common characters to it. Otherwise, we make it through all of the other `strs[1:]` and didn't encounter an uncommon character, then `strs[0][i]` is shared and can be appended to the prefix. After the loop, return prefix - this case would be that the prefix is infact the longest prefix possible because we made it through the length of the shortest string in `strs` and did not enounter any non-shared characters and return early.


```
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if len(strs) == 0:
            return ""
        elif len(strs) == 1:
            return strs[0]

        prefix = ""
        for i in range(0, min(len(s) for s in strs)): # longest possible prefix is the length of the shortest string in the list
            c = strs[0][i] # Take current character from first string
            for j in range(1, len(strs)): # Check the remaining strings for commonality
                if strs[j][i] != c: # If current place in prefix not equal to the expected common character
                    return prefix
            prefix += c
        
        return prefix
```

A smart way of iterating through less strings, found in a Leetcode solution:

Instead of finding the shortest length string through min of all lengths, and then iterating over that length on every string:
1. sort the entire list of strings
2. compare only the first string with the last string.
3. Since alphabetical sorting will essentially order words by their prefix, these two are the 'furthest' prefixes apart. So, the result shared prefix will break in this word (if it has one). 
4. Iterate over the length of the shorter of first/last strings. If the `i`th character is shared, add it to the result.
5. if we find a discrepency, return the current result. otherwise, loop the min length and return the result after loop

```
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        strs = sorted(strs)

        first = strs[0]
        last = strs[-1]
        prefix = ""
        for i in range(min(len(first), len(last))):
            if first[i] != last[i]:
                return prefix
            prefix += first[i]
        return prefix
```