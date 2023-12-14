---
title: "Unique Paths"
date: 2022-07-24
lastmod: 2022-07-24
tags:
- leetcode
- dynamic programming
---

The problem can be found [here](https://leetcode.com/problems/unique-paths/)

# The Problem
There is a robot that is in an `m x n` grid. It is positioned in the top left corner `(0,0)`. Its goal is to make it to the bottom right corner `(m-1,n-1)`. For each move the robot makes, it can only move either one to the right or one down. Return the number of unique paths the robot coud take to get to its target.

# The Approach
We know that for each move, the robot can only move 1 down or 1 to the right. This means that when it reaches the bottom right corner, it must have either come from the tile above of it, or the tile to the left of it. This would tell us that the number of ways to get to the bottom right corner is equal to the sum of the number of ways to reach the tile to the left of it and the number of ways to reach the tile to the right of it. This tells us we should use dynamic programming. We are using the solutions of smaller subproblems in our problem to find the answer we are looking for. We now go through the process of finding the dynamic programming algorithm.

First, we find the base case, which is simple in this case. We start off at position `(0,0)`. We can never go back to this spot, so we know there is only 1 way to get here, so when we are at position `(0,0)`, the number of ways is 1. Now, we need to find the recurrence relation `OPT(i,j)`. We know it needs two parameters because we require the x and y coordinate. If we follow the relation we used to find the number of paths to the bottom right corner, using the number of paths to the tile to its left and above it, we can easily determine our relationship.
$$ OPT(i,j) = OPT(i - 1, j) + OPT(i, j - 1) $$
We use `(i-1,j)`, as that is the coordinate above, and we use `(i,j-1)` as that is the coordinate to the left. Now, with the grid, we will have to take into account the edge cases, where there is no tile either to the left or above. In these cases, we will just use `0`.
With the edge cases, this gives us all we need to write the code.

# Code
```py
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[0 for i in range(n)] for j in range(m)]
        dp[0][0] = 1
        for i in range(m):
            for j in range(n):
                numWays = 0
                if(i - 1 >= 0):
                    dp[i][j] += dp[i-1][j]
                if(j - 1 >= 0):
                    dp[i][j] += dp[i][j-1]
        # print(dp)
        return dp[-1][-1]
```
We have an `m x n` grid, and at each cell, we perform a constant time operation, so our time complexity is `O(mn)`.