---
---


## Problem

## My solution

I got like 90% there on my own. My solution works, but it's suboptimal in execution:

If nums can be evenly divided into K groups, begin the recursive backtrack loop. I am keeping track of an array of differences, which say the remaining sum in this group (sorta like two-sum) but you could also start at 0 and add until they reach the target.

Anyways, in the loop, we know that if our index reaches `len(nums)` we got through the whole list and necessarily solved the problem, so return True. Otherwise, we're not done - try to add the current `nums[i]` to the subsets. At each subset, if the diff remaining in it is greater than or equal to our current number, we can accomodate it. So, try adding it to that subset (ie. subtracting it from what is remaining) and recurring at `i+1`. Either that recursion eventually solves the problem and returns True, or it fails and returns False. If false, `nums[i]` doesn't work in that bucket - so add it back to the remaining difference. As a safeguard, if we ever return False on an empty bucket, we can immediately break, as the buckets following it are also guarunteed to be empty and therefore return False, and we can save duplicate computation. 


```
class Solution:
    def canPartitionKSubsets(self, nums: List[int], k: int) -> bool:
        total = sum(nums)
        if total % k > 0:
            return False
        target = total // k

        subsets = [target] * k

        def recur(i):
            # If we successfully incremented through the list, we solved it
            if i == len(nums):
                return True
            
            for j in range(k):
                if subsets[j] >= nums[i]:
                    # It's possible to add nums[i] to this subset
                    subsets[j] -= nums[i]
                    if recur(i+1):
                        return True
                    subsets[j] += nums[i] # Backtrack if we didn't reach solution

                if subsets[j] == target:
                    # Failed when starting from empty, so anything after this will also fail
                    break

            # If we're here, then the current config isn't possible
            return False
        
        return recur(0)

```

Here's an optimal solution based on Neetcode's implementation:

Get the target subset sum based on `sum(nums)/k`. Create an array to track whether the num at each index has been assigned to a group.

The recursive backtrack call accepts the current index `i`, the number of incomplete groups `k_rem`, and the current group's sum `curr_sum`. If `k_rem==0`, the caller just finished the final group, so we solved the problem and return True. If `curr_sum == target`, the caller just finished a group; start a new group with index checks at the start of list, an empty sum, and `k_rem` minus one. Otherwise, we have at least one incomplete group left to finish, so try to finish it. Iterating from `i` to `len(nums)`, if we encounter a number that isn't already taken and wouldn't overflow the group, do a recursive + backtrack test of adding it, which will either work and eventually solve the problem or fail, at which point unadd it from the group and continue. Like in the previous implementation, if a num ever fails on an empty group, we know nothing is possible for that number and it wont work, so break and return False immediately. You can also think of this conversely; if partitioning is possible, each number will be able to fit into at least one group, and since groups are unordered, a solution should be reachable with each number starting an new empty group. So, if in the current setup a number doesnt fit into any started groups nor can start its own, the configuration must be bad and we need to return False and backtrack (or its overall not possible).

```
class Solution:
    def canPartitionKSubsets(self, nums: List[int], k: int) -> bool:
        target = sum(nums) / k
        taken = [False] * len(nums)

        def recur(i, k_rem, curr_sum):
            if k_rem == 0:
                # We've filled all the groups!
                return True
            
            if curr_sum == target:
                # We've filled a group, start a new one
                return recur(0, k_rem - 1, 0)
            
            for j in range(i, len(nums)):
                # Keep filling the current group
                if taken[j]:
                    # Already in use by this or another group
                    continue
                elif curr_sum + nums[j] > target:
                    # If added to current group we'd exceed target
                    continue
                
                taken[j] = True
                if recur(j+1, k_rem, curr_sum + nums[j]):
                    return True
                taken[j] = False

                if curr_sum == 0:
                    break

            return False

        return recur(0, k, 0)
```