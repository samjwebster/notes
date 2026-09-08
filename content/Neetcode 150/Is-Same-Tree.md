---
tags:
    nc-easy
    nc-binary-tree
    nc-dfs
    nc-recursion
title: Is Same Tree
---

## Problem

You are given two binary trees. Return `True` if both trees are the same (exactly equal structure and node values). 

## Solution

Use a recursive function approach. 
1. If both nodes are `None`/don't exist, return True. Otherwise, at least one node exists.
2. If both exist and share an equal value, recur on both sides. Return True if both are true, else False (one of the children eventually don't match).
3. Else, either one doesn't exist or they have differing values - return False.

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        # Neither exist!
        if not p and not q:
            return True

        if p and q and p.val == q.val:
            # Same val at this position, check symmetrical movement!
            return (
                self.isSameTree(p.left, q.left) and
                self.isSameTree(p.right, q.right)
            )
        else:
            # Either one doesn't exist or they both exist and diff. vals
            return False
```