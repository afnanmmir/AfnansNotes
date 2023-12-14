---
title: "Container with the Most Water"
date: 2022-07-24
lastmod: 2022-07-24
tags:
- leetcode
- arrays
- two pointer
---

The problem can be found [here](https://leetcode.com/problems/container-with-most-water/)

# The Problem
You are given an integer array of length `n`, `height`. There are n vertical line segments drawn such that the endpoints of these line segments are `(i, 0)` and `(i, height[i])`. Given this, find the lines that together with the x-axis will create a container that will hold the most amount of water.

Example:

```height = [1,8,6,2,5,4,8,3,7]```
In this case, if we draw it out, we will see that having one endpoint at index 1 and one endpoint at index 8 (zero indexed), will get us the container with the most water.

# The Approach
We use a two-pointer approach for this problem. We start a left pointer at the 0th index and a right pointer at the rightmost index. In two-pointer problems, with a left and right pointer, we typically iterate until the left and right pointer overlap each other. In this case, we will do the same. At each iteration, we will calculate the volume using our current left and right pointers, and we will update the maximum volume accordingly. Next, we have to decide whether we want to increment the left pointer or decrement the right pointer. We can see that our volume is always limited by the shorter vertical line of the pair. We cannot go over the shorter line, or else we overflow, thus making the shorter line the limiting factor. If we have two lines, it does not do us any good moving the taller of the two inwards because of the fact that the shorter line is the limiting factor. We see that even if the taller line  gets moved into an even taller line, it won't matter because the shorter line is still the limiting factor, and sicne the pointers are moving inwards, moving the taller one inwards will guarantee a volume less than or equal to what we already have. Therefore, we should always move the pointer that is at the shorter vertical line of the two. This will give a chance at increasing the total volume. We perform these iterations until our left and right pointers have overlapped, and then return the maximum volume.

# Code
```py
class Solution:
    def maxArea(self, height: List[int]) -> int:
        left = 0
        right = len(height) - 1
        maxVol = 0
        while(left < right):
            width = right - left
            h = min(height[right], height[left])
            vol = h * width
            maxVol = max(vol, maxVol)
            if(height[left] < height[right]):
                left += 1
            else:
                right -= 1
        return maxVol
```
