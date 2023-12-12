---
title: "Jump Game II"
date: 2022-07-24
tags:
- software
- greedy
enableToc: true
---
The problem can be found [here](https://leetcode.com/problems/jump-game-ii/)

# The Problem
The problem is as follows: given an array of non-negative integers `nums`, you start at the 0th index of the array. Each element `nums[i]` represents the maximum amount of indices you can jump from index `i`. Given the fact that it is always possible to do so, return the minimum number of steps it takes to reach the end.

# Approach
Take the example `[2,3,1,1,4]`. This can be modeled as a decision tree starting with the root being index 0. At index 0, you can choose to take a jump of 1 or a jump of 2. If you take a jump of 1, you end up at index 1, and you can take a jump of 1,2 or 3, and if you take a jump of 2, you can then take a jump of 1, etc. In this tree, we want to find the shortest path to the last index. To do this, we can divide the array into the tree's "levels". The 0th level will contain only the starting point, 0. The next level will contain indices starting from 1 to the furthest possible index you can reach from index 0, $i_1$. Level 2 will then consist of all indices starting at $i_1 + 1$ to the furthest index you can reach from a jump from level 1. This will continue until you reach the level that contains the last index, and we are guaranteed this happens because it is guaranteed we can reach the end.

This is a greedy algorithm, because at each iteration, we are seeing the furthest index we can reach at our current iteration, and choosing that to be the jump we take.
# Code
```java
class Solution {
    public int jump(int[] nums) {
        int l = 0; // The index that will represent the first index in a level
        int r = 0; // index that represents the last index in a level
        int res = 0; // keeps track of the amount of moves made
        while(r < nums.length - 1){ //until right surpasses the end
            int farthest = 0; //will keep track of the furthest distance we can go from our level
            for(int i=l;i<r+1;i++){
                // iterate through all indices at our level and go through their max jumps to see the furthest we can go
                farthest = Math.max(farthest, i + nums[i]); 
            }
            // furthest is the last index of the next level, and right + 1 is the first index of the next level.
            l = r+1;
            r = farthest;
            // increment number of jumps after each iteration.
            res++;
        }
        return res;
    }
}
```
