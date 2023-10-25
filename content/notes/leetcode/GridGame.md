---
title: "Grid Game"
tags:
- software
- greedy
enableToc: true
---
The problem can be found [here](https://leetcode.com/problems/grid-game/)

# The Problem
There is a 0-indexed 2-D array `grid` of dimensions `2 x n` where the value at `grid[r][c]` is the point value at position `(r,c)`. There are two robots who start at position `(0,0)` and are trying to make their way to `(1, n - 1)`. They can only move right and down.

The first robot goes on its path, and every time it visits a cell, it collects the points at that position and leaves that position with 0 points for the second robot. After the first robot is done, the second robot goes on its path.

The goal of the first robot is to **minimize** the amount of points the second robot can earn, and the goal of the second robot is to **maximize** the amount of points it earns on its path.

If both robots play optimally, determine the number of points the second robot will end up with.

# The Approach
The intuitive approach for this problem is for the first robot to collect as many points as it can on its path, and then have the second robot collect as many points as it can, and then return that value. However, this approach ends up being flawed, so a different approach is needed.

If you notice carefully, you will see that because the robots can only move down and to the right, the first robot, no matter what, will collect some amount of points from the first row, then go down to the second row and collect some amount of points until it reaches the end. Then, the second robot will only be able to either collect points from the top right section, or the bottom left section. It can better be seen in this picture, where the red line depicts a possible path for the first robot, and the blue circles show the regions where the second robot can earn its points:
![Grid Game](/notes/images/GridGamePic.png)

Thus, the goal for the first robot will be to go down to the second row at a time such that the amount of points the second robot can earn from the top right or the bottom left is minimum. 

How would this be done efficiently? A good strategy would be to calculate prefix sums for each position on the first row and the second row. A prefix sum at a position `i` would hold the sum of all elements before and at `i`. This preprocessing step will take linear time, and it will allow us to calculate the sum of a subsequence elements in constant time. For example if we want to find the sum of all elements between index `i` and `j`, we wouls simply do `prefix[j] - prefix[i]`.

Following this, some brute force will be necessary. We take all the possible indices that the first robot can possibly go from the 0th row to the 1st row, and calculate the amount of points the second robot can get from the bottom left section or the top right section. The maximum of the two will be the amount the second robot can earn. We will iterate through all the indices for the first robot, and the minimum amount we calculate as the amount the second robot can earn will be where the first robot will change rows. This amount will also be the amount we return, as this will be the amount such that the first robot attempts to minimize the amount earned by the second robot, and the second robot attempts maximize its earnings.

# Code
```py
class Solution:
    def gridGame(self, grid: List[List[int]]) -> int:
        prefix1 = [grid[0][0]]
        prefix2 = [grid[1][0]]
        for i in range(1,len(grid[0])):
            prefix1.append(prefix1[i-1] + grid[0][i])
            prefix2.append(prefix2[i-1] + grid[1][i])
        minMax = prefix1[-1] - prefix1[0]
        for i in range(len(grid[0])):
            group1 = prefix1[-1] - prefix1[i]
            group2 = prefix2[i-1]
            maxForTwo = max(group1, group2)
            # print(maxForTwo)
            minMax = min(minMax, maxForTwo)
        return minMax
```
