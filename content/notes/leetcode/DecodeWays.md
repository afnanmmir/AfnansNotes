---
title: "Decode Ways"
date: 2022-07-24
lastmod: 2022-07-24
tags:
- leetcode
- dynamic programming
---

The problem can be found [here](https://leetcode.com/problems/decode-ways/)

# The Problem
A message containing letters from A-Z can be encoded in the following mapping:
```
A -> '1'
B -> '2'
C -> '3'
...
Y -> '25'
Z -> '26'
```
To decode a message, the digits must be grouped in a valid manner, and then the reverse mappings can be used. There can be multiple ways to decode a group of digits.

For example: `11106` can be decoded in two ways:
- `1 1 10 6 -> A A J F`
- `11 10 6 -> K J F`

Note that no combination with `0` on its own or with `06` can be used because `0` maps to no letter, and `06` is not equivalent to `6`.

Given a string and this mapping, determine the total number of ways it can be decoded.

# The Approach
Lets use an example of `223426` as the group of digits. If we look at the last digit, we can see there are two possibilities: the last digit can either be the end of a two digit number, or it can be a number on its own. There are a few cases we have to check first. For the single digit case, we must check to make sure the digit isn't 0, as 0 is the only digit without a mapping. If it is a 0, then the digit cannot be mapped on its own. For the double digit case, we must check first if the digit before is a 0. If it is a 0, then the digit cannot be mapped as part of a double digit number. If it is not a 0, then it must be checked to make sure the double digit number created is not greater than 26, as no number greater than 26 has a mapping. If either the first digit is a 0, or the double digit number is greater than 26, then the digit cannot be mapped as part of a double digit number. In our case, for the last digit, it is a 6, and the double digit number is 26. Both are mappable, so it can be mapped as both a single and double digit number. 6 is at index 5. Since it can be mapped as both, the number of ways the whole string can be decoded is the number of times the string up to index 3 can be decoded plus the number of times the string up to index 4 can be decoded. This approach lends itself to a dynamic programming approach, as you can see, we are able to solve the overarching problem by using the solutions to broken down sections of the problem. So we can formulate the `OPT` relation:
- Base Case: With 0 digits, there is 1 way to decode the string. With the first digit, there is 1 way to decode the string if it is not a 0. If it is a 0, there are 0 ways to decode the string
- Recurrence Relation for index i:
  - If the digit is single digit mappable and double digit mappable: `OPT(i) = OPT(i - 1) + OPT(i - 2)`
  - If the digit is single digit mappable only: `OPT(i) = OPT(i - 1)`
  - If the digit is double digit mappable only: `OPT(i) = OPT(i - 1)`
  - If the digit is not mappable either way: `OPT(i) = 0`

The value of OPT at the last index is the return value

# Code
```py
class Solution:
    def numDecodings(self, s: str) -> int:
        dp = [0 for i in range(len(s) + 1)]
        dp[0] = 1
        if(s[0] == '0'):
            dp[1] = 0
        else:
            dp[1] = 1
        for i in range(2, len(dp)):
            singleDigit = s[i - 1]
            doubleDigit = s[i-2 : i]
            # print(singleDigit, doubleDigit)
            ways = 0
            if(singleDigit != '0'):
                ways += dp[i - 1]
            if(doubleDigit[0] != '0' and int(doubleDigit) <= 26):
                ways += dp[i-2]
            dp[i] = ways
        # print(dp)
        return dp[-1]
```
This solution runs in `O(n)` time, as we have an `n` length array to track the subproblems, and each subproblem takes a cosntant amount of time to solve.