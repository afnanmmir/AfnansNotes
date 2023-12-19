---
title: "Longest Consecutive Subsequence"
date: 2022-07-24
lastmod: 2022-07-24
tags:
- leetcode
- hashing
enableToc: true
---
The problem can be found [here](https://leetcode.com/problems/longest-consecutive-sequence/submissions/)

# The Problem
Given an array of numbers, return the length of the longest consecutive numbers sequence (i.e: 1,2,3,...).

Example:

```nums = [100, 1, 200, 4, 2, 3]```

In this array, we would return 4, because `1,2,3,4` is the longest sequence of consecutive numbers.

The algorithm must run in `O(n)` time.

# The Approach
With a sorting algorithm, this would be a pretty simple problem, We would sort the array, and increment a counter as long as we have consecutive elements, and reset the counter when the sequence breaks, keeping track of the maximum. However, there is no sorting algorithm with a time complexity better than `O(nlogn)`. Therefore, we need to use a different approach.

When finding a consecutive sequence, there is always a leftmost element of the sequence, in other words, the start of the sequence. This element, in this case, will never have a number that is one less than it. We can use this fact to find all the starts to all the sequences in our array. We can create a set of all the elements in the array. We can then iterate through all the elements in the array. For each element, we check to see if the number right below it is in the set (constant time operation with HashSet). If that number exists, then we are not at the start of a consecutive sequence. If it does not, then the element we are at is the start of a sequence. Once we find an element that is the start of a sequence, we begin counting up, finding all the elements in the consecutive subsequence by checking if numbers following are in the set. Once we reach a number not in the set, we have the length of the sequence, and update the maximum length.

# Code
```py
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        numberSet = set(nums)
        maxLength = 0
        for i in nums:
            isLeftNeighbor = (i-1) not in numberSet
            if(isLeftNeighbor):
                # print(i)
                seqLength = 1
                current = i + 1
                while(current in numberSet):
                    current += 1
                    seqLength += 1
                maxLength = max(seqLength, maxLength)
        return maxLength
```

Although there is a nested loop in this code, it is still `O(n)` time. This is because in aggregate, each element will still only be checked on once, and this operation will take constant time because of our use of the hash set.