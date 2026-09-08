---
tags:
    - nc-easy
    - nc-linked-list
title: Merge Two Sorted Linked Lists
---

## Problem

You are given two linked lists, both of which are sorted by `val` ascending. Merge the two sorted linked lists into one single sorted linked lists and return its root.

Example:

```
a = [1, 2, 4]
b = [1, 3, 5]

res = [1, 1, 2, 3, 4, 5]
```

## Solution

Use a recursive solution to compartmentalize the problem. It's sorta like a recursive two pointer, where the current call scope are the pointers within each list.

At any given moment, you are only considering two provided roots for list A and list B. Do an immediate check for nonexistence edge cases, which could be because invalid input or a list that has been exhausted via the merging process. 

If we're currently comparing two valid roots, positions in list A and B, compare their values. 
- If `listA.val <= listB.val`, move forward with listA as the next node in the combined list. List A's `next` is now the recursive call: `merge(listA.next, listB)`. What follows listA is everything else in listA merged with listB. Of course, if `listA.val > listB.val`, the `else` case, move forward by the same process with list B being the next root and listB's `next` being the `merge(listB.next, listA)`, the remainder of B and list A.

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        
        if not list1:
            return list2
        if not list2:
            return list1
        
        if list1.val <= list2.val:
            list1.next = self.mergeTwoLists(list1.next, list2)
            return list1
        else:
            list2.next = self.mergeTwoLists(list1, list2.next)
            return list2
```