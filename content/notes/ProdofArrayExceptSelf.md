---
title: "Product of Array Except Self"
tags:
- coding
- software
- arrays
enableToc: true
---
This problem can be found [here](https://leetcode.com/problems/product-of-array-except-self/submissions/)
# The Problem
Given an array of numbers `nums`, return an array of numbers `answer` such that `answer[i]` is equal to the product of all elements in `nums` except `nums[i]`. The algorithm must not use the division operation.

# The Approach
Of course, this problem would be easy with the division operation. We would find the product of all the elements, and then divide that product by each element in the array to get the value of answer at each index. But how do we approach this without the division operation.

We see that for `answer[i]` we would take the product of all the elements to the left of `nums[i]`, and multiply it to all the elements to the right of `nums[i]`. Therefore, for each element in `answer`, we can keep track of the left products and the right products, which are the products of all the elements to the left and right of `nums[i]` respectively, and multiply them together to get `answer[i]`. This takes three linear time traversals, and does not use the division operation. To keep track of these products, we create arrays `left` and `right`. We initialize all the elements of both to 1. For left, we start at index 1, and for each index `i`, `left[i] = left[i-1] * nums[i-1]`. For right, we start at the second to last index, and for each index `i`, `right[i] = right[i+1] * nums[i + 1]`. How these are the left and right sums can be seen clearly. After these arrays are produced, we find `answer` with the following: `answer[i] = left[i] * right[i]`. Thus, in each index, we multiply the product of all elements to the left of the index, with the product of all elements to the right of the index to get our answer.

# Code
```py
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        left = [1 for i in range(len(nums))]
        right = [1 for i in range(len(nums))]
        answer = [1 for i in range(len(nums))]
        for i in range(1, len(nums)):
            left[i] = left[i-1] * nums[i-1]
        for i in range(len(nums)-2, -1, -1):
            right[i] = right[i+1] * nums[i+1]
        for i in range(len(nums)):
            answer[i] = left[i] * right[i]
        return answer
```