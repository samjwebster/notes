---
tags: 
    - lc-medium
    - lc-pinterest
    - lc-string
    - lc-hashmap
title: 811. Subdomain Visit Count 
---

## Problem

You are given a list of count-paired domains, which contain an integer number of visits to a domain and the domain itself, such as `1000 discuss.leetcode.com`. This means that `discuss.leetcode.com` was visited 1000 times, and implicitly so were `leetcode.com` and `com`. Given a list like this, return a list of all count-paired domains of all subdomains in any order.

Example:
```
input = ["1000 discuss.leetcode.com", "5 leetcode.com"]
output = ["1000 discuss.leetcode.com", "1005 leetcode.com", "1005 com"]
```


## My solution

Use a dict/hashmap to store the counts hit on each subdomain. Iterate over the list of count-paired domains, extracting the integer count and list of subdomains from the cpdomain string. Iterate over the subdomains, where `subdomains[i:]` represents the full subdomain at that position (for example, in `discuss.leetcode.com`, `i=0` gives the full domain, `i=1` joins just `leetcode.com`, etc). Update these keys in the hashmap by adding count to whatever is there, safely using `dict.get(key,0)+ct`. Finally, return the list of counts in the string format required.


```
class Solution:
    def subdomainVisits(self, cpdomains: List[str]) -> List[str]:
        counts = {}

        for cpd in cpdomains:
            ct, subdomains = cpd.split(' ')
            ct, subdomains = int(ct), subdomains.split('.')
            for i in range(len(subdomains)):
                subdomain = '.'.join(subdomains[i:])
                counts[subdomain] = counts.get(subdomain, 0) + ct
        return [f"{ct} {subdomain}" for (subdomain, ct) in counts.items()]
```