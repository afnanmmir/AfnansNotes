---
title: "Reorder List"
date: 2022-08-19
lastmod: 2022-08-19
tags:
- software
- linked list
enableToc: true
---

The problem can be found [here](https://leetcode.com/problems/reorder-list/)

# The Problem
Given a linked list in the following format:
$$L_0 \rightarrow L_1 \dots \rightarrow L_{n-1} \rightarrow L_{n}$$

Reorder the list so that it reads in the following format:
$$L_0 \rightarrow L_{n} \rightarrow L_{1} \rightarrow L_{n-1} \dots$$
For example, given the list:

`1 -> 2 -> 3 -> 4`

return the list:

`1 -> 4 -> 2 -> 3`

# The Approach
We can break this up into steps. If we notice, in order to do this, we will need to access the second half of this list in reverse order, as the relative order of the items in the second half of the original list are flipped. This means we need to reverse the second half of the linked list. In order to do this, we need to find the middle of the linked list. In order to do this we implement a slow and fast pointer method. The fast pointer will iterate through every other node until it reaches the end, and the slow pointer will through every node until the fast pointer reaches null. The slow pointer will then end up at the middle element. Once the slow pointer is at the middle element, we perform a reverse of this linked list, which should be trivial after completing the [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) problem. We need to keep track of the head of this sub linked list. The solution can be solved like this, but we do run into one problem. If we reverse the second half of the list, the last element in the first half of the list will still be linked to the last element of the second half of the list. This can be seen in the following picture:

![LinkedList](/notes/images/LinkedList.png)

The best way to fix this is severing the connection between the first and second half. We would do this by actually having the slow pointer go to the last element of the first half of the list, and severing the connection by setting the next node link equal to `null` (while also keeping track of the original node after, of course).

Now, we need to insert the elements of the second half list into the first half list accordingly. Lets start going through the process at the beginning of the lists, and we can use the following example:

`1 -> 2 -> 3 -> 4 -> 5 -> 6` which gets transformed into `1 -> 2 -> 3  6 -> 5 -> 4`

We see that in order to make the insertion, we have to set the next node for `1` to `6` and then set the next node of `6` to `2`. However, in order to iterate through both lists, we need to keep track of the original nodes that come after `1` and `6`. This means, we will need 2 temporary nodes. We then repeat this process by updating the current node of the first list equal to the original next node of the head, and the current node of the second list equal to the original next node of its head. This means the general process will be as follows given a `curr1st` node and a `curr2nd` node:
- set a temporary node equal to the next node of `curr1st`
- set a second temporary ndoe equal to the next node of `curr2nd`
- set the next node of `curr1st` to `curr2nd`
- set the next node of `curr2nd` to the first temporary node.
- update `curr1st` to the first temporary node
- update `curr2nd` to the second temporary node
- repeat this until either `curr1st` or `curr2nd` is equal to `null`

This will give us the final result.

# Code
```py
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reorderList(self, head: Optional[ListNode]) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        # Find the last element of the first half of the list
        slow = head
        fast = head.next
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        # sever the connection
        second = slow.next
        slow.next = None
        # Reverse the second half
        prev = None
        curr = second
        while curr:
            temp = curr.next
            curr.next = prev
            prev = curr
            curr = temp
        # Perform the insertions
        first = head
        second = prev
        while first and second:
            tmp1 = first.next
            tmp2 = second.next
            first.next = second
            second.next = tmp1
            first = tmp1
            second = tmp2
```
