---
title: "Permutation in String"
tags:
- coding
- software
- sliding window
---
The problem can be found [here](https://leetcode.com/problems/permutation-in-string/)

# The Problem
Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, or `false` otherwise.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

# The Approach

# The Code

```py
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        s1_letters = {}
        s2_letters = {}
        ret = 0
        if(len(s1) > len(s2)):
            return False
        for i in range(len(s1)):
            if(s1[i] not in s1_letters):
                s1_letters[s1[i]] = 1
                s2_letters[s1[i]] = 0
            else:
                s1_letters[s1[i]] += 1
        for i in range(len(s1)):
            if(s2[i] in s2_letters.keys()):
                s2_letters[s2[i]] += 1
        left = 0
        right = len(s1) - 1
        while(right < len(s2) - 1):
            if(s2_letters == s1_letters):
                ret += 1
            front = s2[left]
            end = s2[right + 1]
            if(front in s2_letters.keys()):
                s2_letters[front] -= 1
            if(end in s2_letters.keys()):
                s2_letters[end] += 1
            left += 1
            right += 1
        if(s2_letters == s1_letters):
            ret += 1
        return ret
```