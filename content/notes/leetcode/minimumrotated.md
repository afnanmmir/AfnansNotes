---
title: "Find Minimum in Rotated Sorted Array"
date: 2023-08-16
lastmod: 2023-08-16
tags:
- software
- arrays
- binary search
enableToc: true
---
The problem can be found [here](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/)

# The Problem
You are given an array that is sorted in ascending order; however, the array has been rotated  so that the minimum element is not at the beginning of the array. For example, for the sorted array `[0, 1, 2, 3, 4, 5]`, if it were rotated 2 times, you would end up with the following array: `[4, 5, 0, 1, 2, 3]`. Given a rotated sorted array, find the minimum element with a time complexity of `O(log(n))`.

# The Approach
Given that this is a problem involving arrays of sorted numbers, and the required time complexity is `O(log(n))`, it is evident that we need to use a binary search approach. There are a few cases that occur when we find the minimum element between the left and right pointers, and we just have to determine what is the right course of action for each of these cases:
- One case we need to take care of is when the middle index is the 0th element in the array. In this case, either the minimum element is at the 0th index, or it is at the first index. We check which index has the smaller of the two values and return that index.
- Another case is when the element at the middle index is greater than the element on its right side. Because it was originally a sorted array, there is only one time this can happen, and that is when the maximum element transitions to the minimum element. Therefore, we know that the minimum element is at the index to the right of the middle index.
- Another case is when the element at the middle index is less than the element on its left side. For the same reason as the last case, this would have to mean the minimum element is at the middle index.
- If it is none of the above cases, then we check whether the element at the middle index is greater than the element at the right most index. If it is, then we know that the minimum element is to the right because it means somewhere between the middle index and the last index, the maximum element transitions to the minimum element. If it is not greater than the element at the right most index, then we know that the minimum element is to the left because it means somewhere between the first index and the middle index, the maximum element transitions to the minimum element.

We iterate through these cases until we find the minimum element, and we return that element.

# The Code
```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        left = 0
        right = len(nums) - 1
        if (len(nums) == 1):
            return nums[0]
        while(left < right):
            mid = (left + right) // 2
            if (mid == 0):
                if (nums[mid] < nums[mid + 1]):
                    return nums[mid]
                else:
                    return nums[mid + 1]
            if (nums[mid] < nums[mid - 1]):
                return nums[mid]
            if (nums[mid] > nums[mid + 1]):
                return nums[mid + 1]
            if (nums[mid] > nums[-1]):
                left = mid + 1
            else:
                right = mid - 1
        return nums[left]
```