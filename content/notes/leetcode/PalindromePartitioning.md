---
title: "Palindrome Partitioning"
date: 2026-01-06
lastmod: 2026-01-06
tags:
- leetcode
- backtracking
enableToc: true
---
The problem can be found [here](https://leetcode.com/problems/palindrome-partitioning/)

# The Problem
Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.

# The Approach
Given that we are trying to find _all possible_ outcomes of something, we can deduce this is a backtracking problem. In this case, the decision to make at each index is whether or not we should partition at that index or not. To create partitions, we need to keep track of start index and end index of partitions. What we want to do is create a partition everytime we see a palindrome, and then recurse on the rest of the string, creating partitions on the rest of the string to create our full palindrome partitioning. We will also want to pop the palindrome off of the list of palindromes afterwards so we get _all possible_ palindrome partitions.

To do this, our decision to create a partition will basically revolve around the condition that our current partition (defined by a start index and end index) is a palindrome. If our current partition is a palindrome, then we can decide to partition on that end index. What happens when we partition?

1. We add the current partition to our list of palindromes that we are tracking
2. We backtrack, setting our start index to the index right after our previous end index was

To actually implement this we don't need to track of an end index. What we do is keep track of a start index, and iterate over all end possible indices. For each end index, we check if that partition from start to end index is a palindrome. If it is, we recurse on it to get the rest of the partitions. Then we pop that partition off of the list to iterate over the rest of the end indices.

Our base case will be when the start index of the partition reaches the end of the string, when there are no more partitions to be made. We then add the list of palindrome partitions to our return list, and return.

# The Code
```py
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        ret_list = []
        def backtrack(cur_list, cur_index):
            if (cur_index >= len(s)):
                ret_list.append(cur_list[:])
                return
            for i in range(cur_index, len(s)):
                if (self.isPalindrome(s, cur_index, i)):
                    cur_list.append(s[cur_index:i+1])
                    backtrack(cur_list, i + 1)
                    cur_list.pop()
        backtrack([], 0)
        return ret_list
    
    def isPalindrome(self, string, start, end):
        temp = string[start:end + 1]
        reverse = temp[::-1]
        return temp == reverse
```