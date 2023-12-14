---
title: "House Robber"
date: 2022-08-07
lastmod: 2022-08-07
tags:
- leetcode
- dynamic programming
enableToc: true
---

The problem can be found [here](https://leetcode.com/problems/house-robber/)

# The Problem
You are a robber that is planning to rob houses along a street. Each house has a certain amount of money you can steal. The only constraint stopping you from robbing all the houses is the fact that adjacent houses have security systems connected, so if you rob two adjacent houses, the police will automatically be contacted. You want to rob the maximum amount of money.

Given the integer array `nums` that gives the amount of money at each house, return the maximum amount of money you can rob that night without alerting the police.

# The Approach

We take a dynamic programming approach to this problem.

First, we have our base cases. If we have one house, we simply rob that house, and if we have two houses, we simply rob the house with more money.

Next, we need our $OPT$ recurrence relation. Say we are at house $i$. We have two options, and we want to see which one will give us the most amount of money. We either do or do not rob the house we are at. If we do not rob the house we are at, then we have achieved the maximum amount we can reach using up to the $i-1$ house. If we do rob the house, this means we would retrieve more money using up to the $i-2$ house and add the amount of money we earn from robbing the house we are at. However, we do not know which of these two options is the optimal solution, so we take the maximum of the two. This is our solution for house $i$, and gives us the $OPT$ relation:
$$ OPT(i) = \max{\{OPT(i-1), OPT(i-2) + nums[i]\}}$$

This gives us enough to write the code

# Code
```java
class Solution {
    public int rob(int[] nums) {
        int[] dp = new int[nums.length];
        if(nums.length == 1){
            return nums[0];
        }
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        for(int i = 2;i < dp.length;i++){
            dp[i] = Math.max(dp[i-2] + nums[i], dp[i-1]);
        }
        return dp[dp.length-1];
    }
}
```

We can also look at an extension of this problem, [House Robber II](https://leetcode.com/problems/house-robber-ii/)

This is the same problem, but the houses are arranged in a circle now. This means that the first house and the last house are now adjacent to each other. Clearly, this is very similar to the original problem. The only difference between this problem and the original is the fact that for any solution, we cannot use both the first and last elements. In order to account for this, we can see that all we need to do is perform the same DP algorithm twice on the array. The first time, we include all elements except the last element. The second time, we include all elements except the first element. By doing them twice like this, we prevent ever using both the first and last element. We then take the maximum of the two, and return that value

# The Code Part II
```java
class Solution {
    public int rob(int[] nums) {
        if(nums.length == 1){
            return nums[0];
        }
        if(nums.length == 2){
            return Math.max(nums[0], nums[1]);
        }
        int dp1[] = new int[nums.length - 1];
        int dp2[] = new int[nums.length - 1];
        dp1[0] = nums[0];
        dp2[0] = nums[1];
        dp1[1] = Math.max(nums[0], nums[1]);
        dp2[1] = Math.max(nums[1], nums[2]);
        for(int i = 2;i < dp1.length;i++){
            dp1[i] = Math.max(nums[i] + dp1[i-2], dp1[i-1]);
        }
        for(int i=2;i < dp2.length;i++){
            dp2[i] = Math.max(nums[i + 1] + dp2[i-2], dp2[i-1]);
        }
        return Math.max(dp2[dp2.length - 1], dp1[dp1.length - 1]);
        
    }
}
```
