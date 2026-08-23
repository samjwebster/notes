---
tags: 
    - lc-medium
    - lc-pinterest
    - lc-set
    - lc-string
title: 36. Valid Sudoku
---


## Problem

Validate a sudoku board. Aka, if there are any dupes in rows, columns, or boxes, return False.

## My solution

Really straightforward, use sets to iterate over the rows, columns, and cells. If there is a duplicate, return False. If we get through everything, we're valid, so return True.

```
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        def dupe(lst):
            return len(row) > len(set(row))

        for i in range(9):
            row = set()
            for j in range(9):
                n = board[i][j]
                if n != '.':
                    if n in row:
                        return False
                    row.add(n)

        for i in range(9):
            col = set()
            for j in range(9):
                n = board[j][i]
                if n != '.':
                    if n in col:
                        return False
                    col.add(n)
        
        for row in range(0, 9, 3):
            for col in range(0, 9, 3):
                box = set()

                for i in range(row, row+3):
                    for j in range(col, col+3):
                        n = board[i][j]
                        if n != '.':
                            if n in box:
                                return False
                            box.add(n)
        return True
```