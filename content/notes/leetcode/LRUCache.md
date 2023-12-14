---
title: "LRU Cache"
date: 2022-10-23
lastmod: 2022-10-23
tags:
- leetcode
- linked lists
- hashmap
enableToc: true
---
The problem can be found [here](https://leetcode.com/problems/lru-cache/)

# The Problem
Design a data structure that follows the constraints of an Least Recently Used (LRU) Cache.
- An LRU cache should be initialized with a given capacity
- `int get(int key)` should return the value of the key if it exists. Otherwise, return -1
- `void put(int key, int val)` should update `key` if `key` exists. If it doesn't exist, add the key-value pair to the cache. If the number of pairs inside the cache exceeds capacity, evict the least recently used key
The functions `get` and `put` should have an average runtime of `O(1)` .

# The Approach
First, in order for the `get` method to have an average runtime of `O(1)`, we will need to use a dictionary/hashmap to store key-value pairs. This will also make `put` have an average runtime of `O(1)` for most cases; however, whenever we need to evict an element because we have run out of space, the dictionary will not be enough for us to remove an element in `O(1)` time. In order to retrieve the least recently used element in `O(1)` time, we can use a doubly-linked list, where the head of the list will be the least recently used element, and the tail element will be the most recently used element. The general rule will be as follows:
- When an element is added, it will be added to dictionary, and it will be added to the end of the Linked List as the most recently used element.
- If a `get` operation is performed on an element, it will be removed from the Linked List at its current position and added back to the end of the Linked List as the most recently used element.
- If during a `put` operation the capacity is exceeded, the element at the head should be removed from the linked list and removed from the dictionary
Using this linked list will allow us to find and remove the least recently used element in constant time. This also requires us to use a Node class that will store the value as well as a previous and next node:

```python
class Node:
    def __init__(self, key, val):
        self.key = key
        self.value = val
        self.prev = None
        self.next = None
```

We also will require methods to remove nodes and insert nodes to our linked list. 
```python
def deleteNode(self, node)

def insertNode(self, node)
```
With both of these methods, the rest of the class becomes simple

# The Code
```python
class Node:
    def __init__(self, key, val):
        self.key = key
        self.value = val
        self.prev = None
        self.next = None

class LRUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = {}
        self.left = Node(0,0)
        self.right = Node(0,0)
        self.left.next = self.right
        self.right.prev = self.left
        
    def removeNode(self, node):
        prev = node.prev
        nxt = node.next
        prev.next = nxt
        nxt.prev = prev
    
    
    def insertNode(self, node):
        prv = self.right.prev
        nxt = self.right
        prv.next = node
        nxt.prev = node
        node.next = nxt
        node.prev = prv

    def get(self, key: int) -> int:
        if(key not in self.cache):
            return -1
        else:
            n = self.cache[key]
            self.removeNode(n)
            self.insertNode(n)
            return n.value

    def put(self, key: int, value: int) -> None:
        if(key in self.cache):
            self.removeNode(self.cache[key])
        self.cache[key] = Node(key, value)
        self.insertNode(self.cache[key])
        if(len(self.cache) > self.capacity):
            lru = self.left.next
            self.removeNode(lru)
            del self.cache[lru.key]
        


# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```
