---
tags: 
    - lc-medium
    - lc-pinterest
    - lc-string
    - lc-trie
title: 720. Longest Word in Dictionary
---

## Problem

Given a list of words representing a dictionary, return the longest word in `words` that can be build one character at a time by other words in `words`. If there is more than one answer, return the one with lower lexicographical order (earlier alphabetically).

Example:
```
words = ["h", "he", "hel", "hell", "hello"]
res = "hello"
```

```
words = ["a", "banana", "apply", "app", "ap", "apple", "appl"]
res = "apple" # comes before 'apply' alphabetically
```

## My solution


Use a trie data structure, also known as a prefix tree. Build this as you iterate through the dictionary of words. Since we need words to be buildable one word at a time, first sort the dictionary by word length. 

For each word in the dictionary, 
1. Starting from the trie root, iterate position according to ascending character in the prefix of the word (`w[:-1]`)
2. If we encounter a character in the prefix `w[:-1]` that is not in the trie, we can break, this word isn't a possible result. Since we ordered the dictionary of words, any word that is a possible result will necessarily be built from other shorter words, which will be in the trie at that point of reading the result. So, if this word has a prefix character that is not in the trie, that means we are definitely missing at least one of its building blocks
3. If we make it through the prefix in the trie without any missing characters, this word is a valid option for res and also a valid building block for the trie
4. Add it to the trie. That is, update the current `trie_pos`, which is `w[-2]` (or root if `len==1`) to have the last character of `w`, `w[-1]`, to be a new possible node in the trie (`{}`). Make sure not to overwrite this character in case it already exists from a previous word. I use `dict.get()` to safely set `trie_pos[w[-1]]` to either an empty dict if doesnt exist or to keep the dict we have thus far if it does exist already. This process ensures that this string can be recognized as a building block for future longer strings that have it in its prefix.
5. After adding the final character to the trie, we can check if it is a better result than the current result we have. If it is longer, overwrite. if it is the same length but lower in lexicographical order (comes earlier alphabetically, or `w < res`), overwrite.
6. Whatever we have at the end is the correct result. Either we didn't find anything so the result is `""`, or we build and descended down the trie to validate some `w` that becomes `res`.



```
class Solution:
    def longestWord(self, words: List[str]) -> str:
        
        trie = dict()

        # Sort by length ascending
        words.sort(key = len)
        res = ""

        for w in words:
            trie_pos = trie # start at the root
            for c in w[:-1]: # increment through all but the last character
                if c not in trie_pos:
                    break # if we can't access the current prefix part in the current trie, this cannot be the res, so stop
                else:
                    trie_pos = trie_pos[c] # otherwise, proceed down the trie
            else: # in a for...else loop, this will execute if we *did not break* out of the for loop. So, that means the prefix at this point is valid
                trie_pos[w[-1]] = trie_pos.get(w[-1], {}) # add the final character to the trie, if it doesnt already exist
                if len(w) > len(res) or (len(w) == len(res) and w < res):
                    res = w
        return res

```