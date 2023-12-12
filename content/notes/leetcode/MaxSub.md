---
title: "Maximum Subarray"
date: 2022-07-30
lastmod: 2022-07-30
tags:
- software
- greedy
enableToc: true
---
The problem can be found [here](https://leetcode.com/problems/maximum-subarray/)

# The Problem
Given an array of integers `nums`, find the contiguous subarray that has the largest sum, and return the sum.

# The Approach
The basic idea here is that you should be iterating through the whole list, keeping track of a running sum, and updating a maximum sum. If the sum ever goes below zero, the running sum should be reset back to zero, and the process will continue. The reason behind this is if we are at an index, and the current running sum is a negative number, then we are better off starting a new subarray at the index we are at rather than adding it to our current sum because it is a negative number. For example:
```nums = [-2,1,-3,4,-1,2,1,-5,4]```
Our first sum here starts at `-2`, then we go to the `1`. We are better off starting a new subarray sum at `1` than continue the sum with the `-2`. Therefore, we reset the sum to 0 before we go to the `1`. This will be done throughout the array.

# Code
```py
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        maxSum = nums[0]
        if(len(nums) == 1):
            return maxSum
        currSum = 0
        i = 0
        while(i < len(nums)):
            if(currSum < 0):
                currSum = 0
            currSum += nums[i]
            maxSum = max(maxSum, currSum)
            i += 1
        return maxSum
```