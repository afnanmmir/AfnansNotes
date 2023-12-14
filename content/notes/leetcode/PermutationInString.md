---
title: "Permutation in String"
date: 2023-06-26
lastmod: 2023-06-26
tags:
- leetcode
- sliding window
---
The problem can be found [here](https://leetcode.com/problems/permutation-in-string/)

# The Problem
Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, or `false` otherwise.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

# The Approach
First, we must check if the length of `s2` is greater than or equal to the length of `s1`. If it is not, then we can return `false` immediately because `s2` cannot contain a permutation of `s1` if it is shorter than `s1`. We then create two dictionaries: one for `s1` that contains all the letters in `s1` and their frequencies in `s1`. The second one will have the same keys, but they will be initialized to `0`. This is to keep track of the letters in `s1`that we encounter in `s2`. We then iterate through the first `len(s1)` characters of `s2` and increment the value of the key in the second dictionary if the character is in the first dictionary. We then create a sliding window of size `len(s1)` and iterate through the rest of `s2`. At each iteration, we check if the two dictionaries are equal. If they are, then we increment the return value. We then remove the character at the front of the sliding window from the second dictionary and add the character at the end of the sliding window to the second dictionary. We then return the return value.

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