---
title: "Longest Palindrome by Concatenating Two Letter Words"
tags:
- software
- strings
- math
enableToc: true
---
This problem was a practice problem I had encountered on Code Signal, so it cannot be found on LeetCode.

# The Problem
Given an array of positive integers `a`, your task is to calculate the sum of every possible `a[i] ˚ a[j]`, where `a[i] ˚ a[j]` is the concatenation of the string representations of `a[i]` and `a[j]` respectively.

For example, if `a = [10, 2]`, thenwe return 1344:
- `a[0] ˚ a[0] = 10 ˚ 10 = 1010`
- `a[0] ˚ a[1] = 10 ˚ 2 = 102`
- `a[1] ˚ a[0] = 2 ˚ 10 = 210`
- `a[1] ˚ a[1] = 2 ˚ 2 = 22`
So the sum is `1010 + 102 + 210 + 22 = 1344`.

# The Approach
There is an obvious approach to solve this, which is to use a nested for-loop to get every possible combination of `(i, j)`, and then concatenate the string representations of `a[i]` and `a[j]` and add them to the sum. However, this approach is `O(n^2)`, which is not ideal, and would cause a time out on Code Signal.

There is a way to bring this to `O(n)`. First, we need to understand how each number will contribute to the final sum. Let's say the length of the array `a` is `l`. There are two cases for each number `a[i]` for how it will be concatenated with the other numbers:
- `a[i]` will be the most significant part of the number (i.e. it will be the first number in the concatenation)
- `a[i]` will be the least significant part of the number (i.e. it will be the second number in the concatenation)

Let's tackle the latter case first. Because we need to consider every case of `i` and `j` where `0 <= i, j < l`, there will be a total of `l` cases for each number in `a`, where `a[i]` is the least significant part of the number. So, for the final sum, each number will contribute `l * a[i]` to the sum for the case where `a[i]` is the least significant part of the number. This comes out to `lower_sum = l*a[0] + l*a[1] + ... + l*a[l-1]`, but this ends up being equal to `l * sum(a)`.

For the former case, it is a little trickier. For each number `a[i]`, there will be, again, `l` cases where `a[i]` is the most significant part of the number. However, when `a[i]` is the most significant part of the number, its contribution to the final sum depends on the number of digits in the least significant part. For example, in the case where `a = [10, 2]`, there are two cases where `10` is the most significant part of the number: `1010` and `102`, but for each case, `10` takes on two different values (`1000`, and `100`) respectively. In order determine the contributions of `10` in the former case, we need to determine the offset each number in the array `a` will cause, which will tell us the total amount each `a[i]` will contribute in the former case. In the example, we can use this method to determine the contrinbutions of `10` in the former case. `10` has 2 digits, so if `10` was concatenated before it, it would make the value of the former `10` be 1000. `2` has 1 digit, so `10` in front of it will be valued at `100`. The general rule would be `a[i] * 10^(number of digits in a[j])`. For each element `a[i]`, the contributions of `a[i]` in the former case would be `a[i] * (10^(number of digits in a[0]) + 10^(number of digits in a[1]) + ... 10^(number of digits in a[l-1]))`. This can be applied to all numbers in `a`, so this can actually be simplified to `upper_sum = sum(a) * (10^(number of digits in a[0]) + 10^(number of digits in a[1]) + ... 10^(number of digits in a[l-1]))`. Then, we can take all the contributions from the former case and add it to the contributions from the latter case to get the final sum. 

In this case, each element only needs to be traversed through once, so the time complexity is `O(n)`, which is much better than `O(n^2)`.
# The Code
```py
def solution(a):
    total_sum = 0
    low_sum = 0
    offset_sum = 0
    for i in range(len(a)):
        low_sum += a[i]
        size = len(str(a[i]))
        offset_sum += 10**size
    total_sum = low_sum * offset_sum + low_sum * len(a)
    return total_sum
```