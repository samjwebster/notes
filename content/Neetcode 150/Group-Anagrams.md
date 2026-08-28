---
tags:
    - nc-medium
    - nc-string
    - nc-sort
title: Group Anagrams
---

## Problem

Given a list of strings, group all anagrams into sublists and return them in any order.

Example:

```
strs = ["act","pots","tops","cat","stop","hat"]
res = [["hat"],["act", "cat"],["stop", "pots", "tops"]]
```

## My solution

Store in a dict where the key is the sorted string (anagram key, all anagrams are the same when sorted) and the value is a list of strings with that sorted key. Iterate over the words, sorting them and converting to string, and appending to that anagram's sublist accoridng to sorted anagram key (or creating the sublist, if first/only in group). Return the values of the dict cast to list.

```
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        groups = dict()
        for s in strs:
            s_sorted = ''.join(sorted(s))
            groups[s_sorted] = groups.get(s_sorted, []) + [s]
        return list(groups.values())
```