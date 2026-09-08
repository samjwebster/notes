---
tags:
    nc-easy
    nc-binary-tree
    nc-recursion
    nc-dfs
title: Subtree of Another Tree
---

## Problem

You are given two trees as input: `root` and `subRoot`. Return `True` if the `subRoot` exists within `root`'s tree structure, otherwise return `False`.

## Solution

Use a recursive DFS approach, combined with the (Is Same Tree)[Is-Same-Tree] solution. 
1. If the `subroot` doesn't exist, return True
2. If the `root` doesn't exist, return False - only True when `subroot` also doesn't exist, which we already accounted for
3. Otherwise, we have a `root` and `subroot` that both have something. 
4. Descend down `root`. 
5. If `isSameTree(root, subroot)`, return True - the `subtree` has been matched at `root`'s position!
6. Otherwise, recur down the left and right children of `root`. The `subroot` structure may yet exist deeper in the tree.

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution: 
    def isSameTree(self, a, b):
        if not a and not b:
            return True
        if a and b and a.val == b.val:
            return (
                self.isSameTree(a.left, b.left) and
                self.isSameTree(a.right, b.right)
            )
        return False

    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        if not subRoot:
            return True
        if not root:
            return False

        if self.isSameTree(root, subRoot):
            # We're currently at a root that is a match to subroot!
            return True
        else:
            # If this root isn't, it might be in a child's strucutre. recur the tree's structure
            return (
                self.isSubtree(root.left, subRoot) or 
                self.isSubtree(root.right, subRoot)
            )
```