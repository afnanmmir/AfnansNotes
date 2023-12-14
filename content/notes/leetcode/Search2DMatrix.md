---
title: "Search A 2-D Matrix"
date: 2023-07-23
lastmod: 2023-07-23
tags:
- leetcode
- binary search
enableToc: true
---
The problem can be found [here](https://leetcode.com/problems/search-a-2d-matrix/).

# The Problem
You are given an `m x n` matrix with the following two properties:
- Each row is sorted in non-decreasing order.
- The first integer of each row is greater than the last integer of the previous row.

Essentially, you have a sorted 2-D matrix. Given an integer `target`, return `true` if `target` is in the matrix, and `false` otherwise.

Your solution must be in `O(log(m*n))` time complexity.

# The Approach
This problem is essentially performing a 2 dimenstional binary search. The first step is to find the row that the target element could be in. Since each row is sorted, we can determine this by checking if the target is greater than or equal to the first element of the row, and less than or equal to the last element of the row. Since the rows themselves are sorted, we can perform binary search on the rows of the matrix to determine which row the target element could be in. We have our `top` pointer and `bottom` pointer. Our middle row will be `(top + bottom) // 2`. If the target element is less than the first element of the middle row, we update bottom. If the target element is greater than the last element of the middle row, we update top. We do this until we find the row where the target is greater than or equal to the first element and less than or equal to the last element, or until top and bottom cross each other. If top and bottom cross each other, then we know that the target element is not in the matrix, and we can return `false`.

Once we find the row the target may reside in, we can perform a simple binary search on the row we have to determine if the element is present in the matrix, and return `true` if it is, and `false` otherwise.

# The Code
```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        def searchColumns(matrix, target):
            top = 0
            bottom = len(matrix) - 1
            while (top <= bottom):
                mid = (top + bottom) // 2
                first = matrix[mid][0]
                last = matrix[mid][-1]
                if (target >= first and target <= last):
                    return mid
                elif (target < first):
                    bottom = mid - 1
                elif (target > last):
                    top = mid + 1
            return -1
        def searchRow(row, target):
            left = 0
            right = len(row) - 1
            while (left <= right):
                mid = (left + right) // 2
                if (target == row[mid]):
                    return mid
                elif(target < row[mid]):
                    right = mid - 1
                else:
                    left = mid + 1
            return -1
        row = searchColumns(matrix, target)
        if (row == -1):
            return False
        ind = searchRow(matrix[row], target)
        return ind != -1 
```
