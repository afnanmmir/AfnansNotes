---
title: "Longest Common Subsequence"
date: 2022-07-29
lastmod: 2022-07-29
tags:
- software
- dynamic programming
enableToc: true
---

The problem can be found [here](https://leetcode.com/problems/longest-common-subsequence/)

# The Problem
Given two strings `text1` and `text2`, find the length of the longest common subsequence, where a subsequence of a string is a new string generated from the original string with some number of characters (could be none) deleted and order preserved. If no common subsequence exists, return 0.

# The Approach
Lets start at the last character of each string. If these characters are the same, then we can say that the length of the longest common subsequence is equal to 1 plus the longest common subsequence up to the second to last character. However, if they are not the same, we have three different options. The longest common subsequence can exist using all of the first string and up to the second to last character of the second string, it can exist using all of the second string and up to the second to last character of the first string, or it could only use up to the second to last characters for both strings. However, we don't know which one it will be, so we must take the maximum of both. This leads itself to be a dynamic programming problem.

We start with the base case. If we use no characters from either string, then the longest common subsequence will be of length 0.

Next, we define the $OPT$ relation. This is a case where we have to create a two dimensional table, as we have to keep track of the index of the first string and the index of the second string. This means, we will have an `m x n` table, where m is the length of `text1`, and n is the length of `text2`. For $OPT(i,j)$, we know that if `text1[i]` is equal to `text2[j]`, then the longest common subsequence is equal to $1 + OPT(i-1,j-1)$. However, if this is not the case, we have the three options that were listed above. This translates to:
$$ OPT(i,j) = \max{\{OPT(i,j-1), OPT(i-1,j), OPT(i-1,j-1)\}}$$
Thus, we have our full OPT recurrence relation and can write the code.

# Code
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length();
        int n = text2.length();
        int[][] dp = new int[m + 1][n + 1];
        for(int i = 0;i < m + 1; i++){
            dp[i][0] = 0;
        }
        for(int i = 0;i < n + 1; i++){
            dp[0][i] = 0;
        }
        for(int i = 1;i < m + 1;i++){
            for(int j = 1;j < n + 1;j++){
                if(text1.charAt(i - 1) == text2.charAt(j - 1)){
                    dp[i][j] = dp[i-1][j-1] + 1;
                }else{
                    dp[i][j] = Math.max(Math.max(dp[i-1][j], dp[i][j-1]), dp[i-1][j-1]);
                }
            }
        }
        return dp[m][n];
    }
}
```