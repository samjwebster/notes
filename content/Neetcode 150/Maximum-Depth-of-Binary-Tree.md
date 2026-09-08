---
tags:
    - nc-easy
    - nc-binary-tree
title: Maximum Depth of Binary Tree
---

## Problem

Given a binary tree (nodes that descend with one possible child to the left and one possible child to the right), return the maximum depth on that binary tree.

## Solution

There are a handful of ways to solve this using various tree navigation algorithms. I opted for a recursive approach, which is functionally identical to DFS but syntactically simpler.

If the current node is null/None, return 0 as it can't contribute to depth if it doesnt exist. Otherwise, we are indeed at a node; sum 1 (curr node) and the maximum depth when recurring down the left and right children. 

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0

        return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right))
```