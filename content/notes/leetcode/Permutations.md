---
title: "Permutations"
date: 2026-01-06
lastmod: 2026-01-06
tags:
- leetcode
- backtracking
enableToc: true
---
The problem can be found [here](https://leetcode.com/problems/permutations/)
# The Problem
Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

# The Approach
Given that this is a problem that we are trying to essentially find _all possible combinations_ of something, we can deduce it is a backtracking problem. In the case of a backtracking problem, we essentially perform a DFS on the decision tree/space of the problem. What this means is we have a series of choices to make that each have some sort of constraint on them, and we essentially trace down the outcome of each of these choices when we reach a certain base case. Once we have reached that base case, we can return.

For this problem specifically, we are trying to create permutations of a list of numbers, where order matters. Essentially, we have to make a series of $n$ choices, where $n$ is the number of elements in the list. For each choice, we have to choose a number in the list to add to the permutation we are trying to generate. The constraint is it cannot be a number that we have already used. So, for each choice, we can iterate through all the numbers that we _can_ use, and recurse on that decision to reach all the permutations. In order to ensure we do not use a number twice, we need to keep track of all the numbers we have used. We can do this using a set that we add numbers to once we have used them.

The steps for the backtracking would go as follows:
1. We have the partial permutation that we are building. (This could be an empty array, a full permutation, or some partial generation)
2. We check if the number of elements is equal to the full list of numbers. If it is, we add it to our list of permutations
3. We iterate over all the numbers in the list of numbers
4. If the number is in the visited set, we skip the iteration
5. If the number is not in the visited set, we add it to the permutation and recurse.
6. After the recursion, we remove the element from the permutation and remove it from the visited set.

# Code
```py
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        ret_list = []
        used = set()
        def backtrack(cur_list):
            if (len(cur_list) == len(nums)):
                ret_list.append(cur_list[:])
                return
            for i in range(len(nums)):
                if (nums[i] in used):
                    continue
                
                cur_list.append(nums[i])
                used.add(nums[i])
                backtrack(cur_list)

                cur_list.pop()
                used.remove(nums[i])
        backtrack([])
        return ret_list
```