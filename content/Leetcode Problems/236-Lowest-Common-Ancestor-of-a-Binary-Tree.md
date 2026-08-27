---
tags: 
    - lc-medium
    - lc-pinterest
    - lc-binary-tree
    - lc-dfs
    - lc-recursion
title: 236. Lowest Common Ancestor of a Binary Tree
---

## Problem

You are given a binary tree and two nodes `p` and `q`. Find the lowest common ancestor shared by both nodes in the tree. That is, if you draw paths from root to `p` and to `q`, what is the furthest node in the binary tree that is along both paths? The target nodes can be the shared ancestor themselves.

Example:
```
bin_tree =
      3
    /    \
   5      1
  / \    / \
 6   2  0   8
    / \
   7   4

p = 5
q = 1
ancestor = 3
```

## My solution

Had to work a bit with Gemini to guide me to the right implementation but my intuition was pretty correct.

Use recursive DFS to iterate down the tree, keeping a record of the path you've currently taken from the root. If you find one of the target nodes, save a copy of the path for that node. Stop the recursion once both nodes are found. Finally iterate over both paths, tracking the deepest shared node until a divergence is found, and return that final shared ancestor.

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        
        p_path = None
        q_path = None

        def recur(pos, path):
            nonlocal p_path, q_path

            if pos == None:
                # Outside the tree (prob from a branch left/right == None)
                return
            elif p_path and q_path:
                return

            path.append(pos)

            # Make a copy if we found one of the paths
            if pos == p:
                p_path = list(path)
            elif pos == q:
                q_path = list(path)

            # Recur
            recur(pos.left, path)
            recur(pos.right, path)

            # Backtrack after recursion
            path.pop() # Remove node

        recur(root, []) # this will find both paths to p and q as sets

        ancestor = None
        for a, b in zip(p_path, q_path):
            if(a == b):
                ancestor = a
            else:
                break
        return ancestor
```



