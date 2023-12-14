---
title: "Course Schedule"
date: 2023-06-26
lastmod: 2023-06-26
tags:
- leetcode
- graph
---
The problem can be found [here](https://leetcode.com/problems/course-schedule/).

# The Problem
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [a_i, b_i]` indicates that you must take course `b_i` first if you want to take course `a_i`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

# The Approach
This problem is a classic topological sort problem, however in this case, we do not need to return the actual sort of the courses, we just need to make sure that it is possible to perform the topological sort. We can think of each course as a node in the graph, and for each pair in `prerequisites`, we can draw a directed edge from `b_i` to `a_i`. To form a model of the graph, we will create a dictionary, where the key is the node, and the value is the adjacency list of the node. We also will need to create a dictionary to record the number of in degrees of each node. We then iterate through the list of prerequisites. For each prerequisite, the source node will be the element in the first index, and the destination node will be the element in the zeroth index. We also increment the in degree of the destination node.

We then perform the following topological sort algorithm:
- Find a node with an in degree of 0.
- Add this node to the set of visited nodes.
- Remove this node from the graph.
- Update the in degrees of the nodes that this node points to.
- Repeat until there are no more nodes with an in degree of 0.

If the number of visited nodes at the end does not equal the number of nodes in the graph, then we know that there is a cycle in the graph, and we cannot perform a topological sort and cannot take all of the courses. Otherwise, we can take all of the courses.

# Code
```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        out_graph = {i:[] for i in range(numCourses)}
        in_degrees = {i:0 for i in range(numCourses)}
        can_finish = True
        visited = set()
        for courses in prerequisites:
            out_graph[courses[1]].append(courses[0])
            in_degrees[courses[0]] += 1
        while (len(visited) < numCourses):
            next_nodes = [i for i in in_degrees if in_degrees[i] == 0]
            if (len(next_nodes) <= 0):
                return False
            next_node = next_nodes[0]
            visited.add(next_node)
            for i in out_graph[next_node]:
                in_degrees[i] -= 1
            del(out_graph[next_node])
            del(in_degrees[next_node])
        return True
```