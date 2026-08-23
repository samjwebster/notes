---
tags: 
    - lc-medium
    - lc-pinterest
    - lc-backtracking
    - lc-dfs
title: 79. Word Search
---

## Problem

You have a 2D array of characters and a target word. Return true if this word exists in the 2D array as a non-overlapping travelable path of adjacent cells (vertical or horizontal neighbors).

Examples:
```
board = [
    ["A*", "B*", "C*",  "E"],
    ["S",  "F",  "C*",  "S"],
    ["A",  "D*", "E*",  "E"]]
word = "ABCCED"
res = true
```

## My solution

Initial attempt, this works and is pretty good but i do some recommended improvements in the second one. But the principal is the same. Do a DFS recursion starting at positions in the board where the character is equal to the starting letter of the string. In the recursive function:
1. First, check for failure cases: if the `(i, j)` pos is out of bounds, or the character at the cell is wrong for what's remaining in the string. 
2. If we pass those, this cell is valid for progressing the path. Update what's remaining in the string, and if we completed the string return `True`. 
3. If we aren't done yet, add the position to the seen set.
4. Recur on all adjacent cells
5. Remove the current position from seen, as it's a shared set.
6. Return what was found

```
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        
        def recur(rem_word, seen, i, j):
            if(i < 0 or i == len(board) or j < 0 or j == len(board[0])):
                return False # End this path, (i, j) is OOB
            if(board[i][j] != rem_word[0]):
                return False # End this path, not right character
            elif((i, j) in seen):
                return False # End this path, right character but already visited cell
            
            # This cell is good! Update remaining word
            rem_word = rem_word[1:] if len(rem_word) > 1 else ""
            if not rem_word:
                return True # We've found the whole word!
            
            # Add it to visited
            seen.add((i, j))

            # Recur to aadjacent cells
            found = any([
                recur(rem_word, seen, i-1, j),
                recur(rem_word, seen, i+1, j),
                recur(rem_word, seen, i, j-1),
                recur(rem_word, seen, i, j+1)
            ])

            seen.remove((i, j))
            return found

        starting_positions = []
        for i in range(len(board)):
            for j in range(len(board[i])):
                if board[i][j] == word[0] and recur(word, set(), i, j):
                    return True

        return False
```

Second attempt with some suggested changes according to good solutions. Same principles as above, recur on positions that match the first letter. In the recursive function:
1. Now tracking index in the word. If we have `k` equal to the target length, we've found a full path so return `True`
2. Otherwise, we're not done. Check for the same failure cases (OOB, wrong char) and return `False` if we fail
3. Update the board to reflect this position as visited. Future recursive checks, if they try that cell, will definitely fail since we've set it to a character outside of `word`'s possible vocabulary
4. Recur on adjacent cells one at time, joining result with or. 
5. Return the changed board to the word character at `k` for other branches of the recursion
6. Return what we've found


```
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        
        def recur(i, j, k):
            if k == len(word):
                return True
            
            if(
                i < 0 or i >= len(board) or # OOB i
                j < 0 or j >= len(board[0]) or # OOB j
                board[i][j] != word[k]): # Wrong character
                return False # End the path

            # This cell is valid! Update remaining word and board
            board[i][j] = "."

            # Recur to aadjacent cells
            found = (
                recur(i-1, j, k+1) or
                recur(i+1, j, k+1) or
                recur(i, j-1, k+1) or
                recur(i, j+1, k+1)
            )

            board[i][j] = word[k]
            return found

        for i in range(len(board)):
            for j in range(len(board[i])):
                if board[i][j] == word[0] and recur(i, j, 0):
                    return True

        return False
```