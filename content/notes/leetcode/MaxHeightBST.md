---
title: "Maximum Depth of Binary Tree"
tags:
- coding
- software
- binary trees
enableToc: true
---
This problem can be found [here](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

# The Problem
Given a binary tree, we need to find the maximum depth of the tree, where the depth of a path is the number of nodes along a path from the root to a leaf node.

# The Approach
Most problems to do with binary trees lend itself to a recursive solution, and this one is no different. This can be seen intuitively, as if we want to find the maximum depth from the root node, we have `1` for the root node, and to get to the maximum depth, we either have to take the left or the right child. We do not know which one we have to take, so we take the maximum. This means the maximum depth is 1 added to the maximum of the maximum depths of the left and right children of the root. But how do we find the maximum depths of the children? We go through the same process, and we continue this down the tree. This clearly is a recursive process. Lastly, what would the base case be? We know that if we have a `null` node, we would have a depth of 0, and if we have only one root node, we have a depth of 1. These are our base cases, but looking carefully, it can be seen that the case of one root node can actually be taken cared of by the recursive case (look at the code to follow this). Thus, we have our relationship:
1. If we have a null node, the depth is 0
2. If we have a non-null node, the depth is 1 + max(depth(leftChild), depth(rightChild))

That ends up being the entire algorithm. It is a very simple algorithm to code, but takes some time to think about, which is the case for many recursive binary tree problems.

# Code
```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```