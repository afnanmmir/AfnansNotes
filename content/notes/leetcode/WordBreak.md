---
title: "Word Break"
date: 2022-08-22
lastmod: 2022-08-22
tags:
- leetcode
- dynamic programming
---

The problem can be found [here](https://leetcode.com/problems/word-break/)

# The Problem
Given a string `s` and a dictionary of words `wordDict`, return true if the string s can be built out of words in the dictionary only.

The same word in the dictionary can be used more than once

Example:
```
s = leetcode
wordDict = ['leet', 'code']
output = true
```
The word `leetcode` can be created by concatenating `leet` and `code`.

# Approach
This problem can be solved using dynamic programming. The basic premise will be the following: we will check every substring starting at the beginning of the string and see whether it can be created by concatenating words in the dictionary, and for each substring, we can use the results of smaller substrings to help our check.

In order to do this, we will need an OPT table. This will be a one dimensional table that will be of length `s.length() + 1` to account for the empty string as well. The base case is simple. For an empty string, we should return 1. This is because we can simply use none of the words in the dictionary. 

Now, we must find the $OPT$ relation for any index `i` ($OPT(i)$). For this, what we do is use it as an ending index, and iterate a second index, `j`, from 0 to `i`. We check the substring `s[j:i]`, and determine if it is in the dictionary or not. If it is, that means we can create a word from it, and all we need to check is if `s[0:j-1]` can be created by words in the dictionary. This will be held in the $OPT$ table at index `j-1` (keep in mind that because there is an index where we account for the empty string, the OPT table in the code will be off by one.). If $OPT(j-1)$ is true and `s[i:j]` is in the dictionary, then we can set $OPT(i)$ to true. If this doesn't occur for any `j` from 0 to `i`, then the entry will be set to false. We do this iteration process for all indices `0` to `s.length() - 1`. We then return the result at the last index, and this will tell us if we can create the whole string using the dictionary.

# Code
```py
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        dp = [0 for i in range(len(s) + 1)]
        wordDictSet = set(wordDict)
        # print(wordDictSet)
        dp[0] = True
        for i in range(1, len(dp)):
            for j in range(0, i + 1):
                # print(j,i)
                if(s[j:i] in wordDictSet and dp[j] == 1):
                    dp[i] = 1
        return dp[-1] == 1
```