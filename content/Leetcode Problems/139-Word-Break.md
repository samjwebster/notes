---
tags: 
    - lc-medium
    - lc-pinterest
    - lc-trie
    - lc-dfs
title: 139. Word Break
---

## Problem 

You are given a string `s` and an array of words `wordDict` representing an available dictionary. Return `True` if it is possible to separate `s` with spaces into a series of valid words from `wordDict`. 

Examples:
```
s = "leetcode", wordDict = ["leet", "code"]
res = True # "leet code"
```

```
s = "catdogcat", wordDict = ["cat", "dog"]
res = True # "cat dog cat"
```

```
s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
res = false
```

## My Solution

My intuition was pretty spot on here! Maybe it's only because I solved it, but this was a fun problem.

First step is to build a trie using `wordDict`. Flag the end of words. This will allow us to easily test available words as we traverse the positions of the string.

Now, use recursion and DFS to test ways to separate the string into dictionary words. In `recur(i)`, the input `i` denotes the string position we want to try starting a word. First check quick exit conditions: if `i == len(s)`, that means the previous call found a word that ended exactly at the end of the string, so we can immediately return true. Otherwise, check the memo hashmap to see if a different DFS path already solved this starting position in the stirng, and return it if so. 

Otherwise, we need to search the trie! Starting at the trie root, progess down the trie character by character from the starting position `i`, until we run out of characters in the string. At each trie position, check the flag to see if the current position is the end of a dictionary word. If it is, great - we can try ending here and continuing the DFS. If that succeeds, that means we solved the problem because it was able to progress all the way down! But if word breaking at that word doesn't work, continue progressing the characters and trying the recursive search at possible positions. If we exhaust everything starting at `i` and still no solution, return false - a solution doesn't exist with a word starting at `i`. 

In the main func, return the value of `recur(0)`, if there's a solution then the DFS will find it!


```
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        
        trie = {}
        flag = "#" # denotes is word
        for w in wordDict:
            trie_pos = trie
            for c in w:
                trie_pos[c] = trie_pos.get(c, {})
                trie_pos = trie_pos[c]
            trie_pos[flag] = True

        i = 0
        memo = dict()
        def recur(i):
            if i == len(s):
                return True
            elif i in memo:
                return memo[i]

            trie_pos = trie
            curr_i = i
            while curr_i < len(s):
                c = s[curr_i]
                if c not in trie_pos:
                    break
                trie_pos = trie_pos[c]
                
                if flag in trie_pos and recur(curr_i+1):
                    return True
                
                curr_i += 1 # Otherwise, continue

            memo[i] = False
            return False
        
        return recur(0)
```