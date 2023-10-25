---
title: "Diameter of a Binary Tree"
tags:
- software
- binary-trees
enableToc: true
---

The problem can be found [here](https://leetcode.com/problems/diameter-of-binary-tree/)

# The Problem
Given the root of a binary tree, return the diameter of the tree.

The diameter is the length of the longest path between any two nodes in the tree, where the length of a path is the number of edges in that path.

# The Approach
First, we can think of the brute force method. In this case, it would be to visit each node, and see how far you can go to the left and how far you can go to the right, and add the two lengths together. We would do this recursively, as to find the lengths to the left and right of the root, you can use the lengths to the left and the right of the left child and the right child. You would do this for every node and return the maximum value you find. However, this is somewhat inefficient.

To make this more efficient, we can use a bottom up approach. We can start at the leaf nodes, and find the diameter at the leaf nodes, and work our way up. To find the diameter/longest path at each node, we need the longest path we can find to the left and add it to the longest path we can find to the right of the node. Another way of saying this, is we need the height of the left subtree and the height of the right subtree (In this case, the height of a tree is defined as the number of edges in the longest path from the root to a leaf). Because we need the heights from the subtrees to calculate the diameter, the height of the node is what we will be returning from our recursive function. We will then add the two heights together. We will then also add 2 to this value, because there are the two edges that connect the left and right subtree to the current node we are at. With this, we have the longest path from a node, and we can keep track of the longest path we find in a global variable, and update it correspondingly.

With this information, we can build our recursive function. Again, this function will return the height of the tree starting at the node we are at, as this is what will be needed at each step. It will also be updating the global longestPath variable, that will keep track of our diameter.

First we need a base case. To do this, we first need to realize that the height of a tree with only the root node is 0 because there are no edges. This means that for the base case, where the node is null, the height will actually be -1, so we would return -1. We then need to calculate the longest path. We do this by finding the height of the left subtree recursively, and finding the height of the right subtree recursively. We then add the two together and add 2. We will then compare this value to our global longestPath variable, and update it if needed. Lastly, we need to return the height of the node we are at. This will be 1 plus the maximum of the height of the left subtree and the height of the right subtree, as this will be the longest path between our node and a leaf. This concludes our recursive function. Lastly, we return our global variable.

# Code
```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        longestPath = [0]
        
        def diameterHelper(root):
            if(root == None):
                return -1
            leftHeight = diameterHelper(root.left)
            rightHeight = diameterHelper(root.right)
            height = 1 + max(leftHeight, rightHeight)
            diameter = leftHeight + rightHeight + 2
            longestPath[0] = max(longestPath[0], diameter)
            return height
        
        diameterHelper(root)
        return res[0]
```