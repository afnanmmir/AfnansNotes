---
title: "Longest Substring of All Vowels in Order"
tags:
- coding
- software
- sliding window
---
The problem can be found [here](https://leetcode.com/problems/longest-substring-of-all-vowels-in-order/)

# The Problem
A string is considered beautiful if it satisfies the following conditions:
- Each of the 5 English vowels ('a', 'e', 'i', 'o', 'u') must appear at least once in it.
- The letters must be sorted in alphabetical order (i.e. all 'a's before 'e's, all 'e's before 'i's, etc.).

For example, strings "aeiou" and "aaaaaaeiiiioou" are considered beautiful, but "uaeio", "aeoiu", and "aaaeeeooo" are not beautiful.

Given a string word consisting of English vowels, return the length of the longest beautiful substring of word. If no such substring exists, return 0.

A substring is a contiguous sequence of characters in a string.

# The Approach

# The Code
```py
class Solution:
    def longestBeautifulSubstring(self, word: str) -> int:
        vowels = ['a', 'e', 'i', 'o', 'u']
        if (len(word) < 5):
            return 0
        left = 0
        right = 1
        index = 0
        max_length = 0
        while left < len(word) and right < len(word):
            index = 0
            if(word[left] != 'a'):
                left += 1
            else:
                right = left + 1
                prev = vowels[index]
                index = 1
                while(right < len(word) and (word[right] == prev or (index < len(vowels) and word[right] == vowels[index]))):
                    if(index >= len(vowels)):
                        length = right - left + 1
                        max_length = max(max_length, length)
                    if(index < len(vowels) and word[right] == vowels[index]):
                        prev = vowels[index]
                        index += 1
                        if(index >= len(vowels)):
                            length = right - left + 1
                            max_length = max(max_length, length)
                    right += 1
                left = right

        return max_length
```