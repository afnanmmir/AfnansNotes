---
title: "Longest Palindrome by Concatenating Two Letter Words"
tags:
- coding
- software
- arrays
- strings
- maps
enableToc: true
---
The problem can be found [here](https://leetcode.com/problems/longest-palindrome-by-concatenating-two-letter-words/description/).

# The Problem
You are given an array of strings `words`. Each element of this array consists of two letters from the English alphabet.

Create the longest possible palindrome by selecting elements from the array and concatenating them in any order, where each element can only be used once. Return the length of the longest palindrome you can create. If it is impossible to create a palindrome, return `0`.

# The Approach
To solve this problem, it is important to distinguish between two different types of two letter combinations that are possible: ones that are not palindromes themselves (i.e `lc` or `ab`) and those that are (i.e `xx`).

To solve this problem, we first deal with the elements that follow the former case. For these elements, to create a palindrome with them, you need to find a complementary element that is the exact opposite of the element (i.e if you have element `lc`, you need to find element `cl`). To do this, we can create a dictionary that contains all the elements in the array mapped to their frequency. Then we iterate back through the array, and for elements that are not palindromes, we see if the complementary element exists, _and_ make sure there is one available to take. Remember that each element can only be used once. This is why we need to keep track of the frequencies. Once a pair is found, the count for each of these elements is decremented by 1, and the length of the palindrome is incremented by 2.

Next is the harder part, which is dealing with the elements that are palnidromes themselves. First thing you want to do is to find the frequencies of each of these elements, similar to how it is done for non-palindrome elements. Next, to decide how they are used, first, you want to find the element with the largest frequency that is odd. This is because if the frequency is even, then the optimal way to use the element is to create pairs such that one element goes in the first half of the palindrom and the other element goes in the second half (e.g. if you have `arr = ["gg", "gg", "lc", "cl"]`, then your optimal solution is `"gglcclgg"`). However, if the frequency  is odd, then it can only be used in one way, where one of the elements is the middlemost element of the string, and pairs are created out of the rest to put on either side of the middle element (e.g. if you have `arr = ["xx", "xx", "lc", "cl", "xx"]`, then your optimal solution is `"lcxxxxxxxxcl"`). However, this can only be used once in the palindrome, so you must find the element with the largest frequency that is odd. Once you find this element, you can use all of its elements. Then, for the rest of the two letter palindromes, you will create as many pairs as you can, and if pairs cannot be created, then they will not be used at all. This means if you have an element `"gg"` that has a frequency of 1, and it is not the element with the largest frequency that is odd, then it will not be used at all.

If you take all the pairs you created, multiply it by 4 (because each pair has 4 letters), then multiply the element with the largest frequency that is odd by 2, then multiply the number of pairs of non-palindrome two letter elements you found, then add them up, this will be the length of the longest palindrome you can create.

# The Code
```python
class Solution:
    def longestPalindrome(self, words: List[str]) -> int:
        nonpal_dict = dict()
        middle_dict = dict()
        longest = 0
        print(len(words))
        for i in range(len(words)):
            string = words[i]
            reversed_word = string[::-1]
            if (string == reversed_word):
                if (string not in middle_dict.keys()):
                    middle_dict[string] = 1
                else:
                    middle_dict[string] += 1
            elif (reversed_word in nonpal_dict.keys() and nonpal_dict[reversed_word] > 0):
                nonpal_dict[reversed_word] -= 1
                longest += 4
            else:
                if(string not in nonpal_dict.keys()):
                    nonpal_dict[string] = 1
                else:
                    nonpal_dict[string] += 1
        values = [middle_dict[x] for x in middle_dict.keys()]
        odds = [x for x in values if x % 2 == 1]
        max_odds = 0
        if (len(odds) > 0):
            max_odds = max(odds)
            values.remove(max_odds)
        num_pairs = [x // 2 for x in values]
        sum_of_pairs = sum(num_pairs) * 4
        return longest + sum_of_pairs + max_odds*2
```