---
tags: 
    - lc-medium
    - lc-pinterest
    - lc-trie
title: 208. Implement Trie (Prefix Tree)
---

## Problem

A trie, or prefix tree, is a data structure that stores words in a dictionary according to their sequence of characters. This can be used to efficiently store, retrieve, and query a set of strings, and has various applications including spellchecking and autocomplete. Implement this data structure, with `init`, `insert`, `search`, and `prefix` methods.

## My solution

For holding the character data itself, the trie is primarily driven by a hashmap that progressively stores available paths of characters. This can be illustrated in the insert function. Starting at the root, the base `self.trie`, iterate over the word. For each character `c`, add it at the position in the trie (being sure not to overwrite what is already stored there, if anything), and descend into that position. After adding all characters, flag the position in the trie as being the end of a word.

Search and startsWith work nearly identically, both using the exact same logic to descend the trie character-by-character along the provided string. In both cases, if we ever encounter a missing character in the trie, return False. For `search`, after we find that the string exists in the trie, return whether or not its final position has the flag denoting the end of a word (for example, "book" would appear in a character chain created by "bookcase", but "book" itself might not be a word). For prefix, we just want to know if it's there at all, so return True.

class Trie:

    def __init__(self):
        self.trie = {}
        self.flag = "$" # an out-of-vocab flag to denote that the position is the end of a word

    def insert(self, word: str) -> None:
        trie_pos = self.trie
        for c in word:
            trie_pos[c] = trie_pos.get(c, {})
            trie_pos = trie_pos[c]
        trie_pos[self.flag] = True

    def search(self, word: str) -> bool:
        trie_pos = self.trie
        for c in word:
            if c in trie_pos:
                trie_pos = trie_pos[c]
            else:
                return False
        return self.flag in trie_pos

    def startsWith(self, prefix: str) -> bool:
        trie_pos = self.trie
        for c in prefix:
            if c in trie_pos:
                trie_pos = trie_pos[c]
            else:
                return False
        return True