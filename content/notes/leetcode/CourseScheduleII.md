---
title: "Course Schedule"
tags:
- coding
- software
- graph
---
The problem can be found [here](https://leetcode.com/problems/course-schedule-ii/).

# The Problem
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [a_i, b_i]` indicates that you must take course `b_i` first if you want to take course `a_i`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return the ordering of the courses you should take to finish all the courses. If it is impossible to finish all courses, return an empty array. If there are multiple correct orders, return any of them.

# The Approach
This problem is almost identical to [Course Schedule](/notes/CourseSchedule.md), except that in this instance, we need to actually return the topological sort of the graph. Everytime we find the node with an in degree of 0, we add it to the topological sort list. If we cannot find a node with an in degree of 0, then we return an empty list.

# Code
```python
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        out_graph = {i:[] for i in range(numCourses)}
        in_degrees = {i:0 for i in range(numCourses)}
        can_finish = True
        visited = set()
        path = []
        for courses in prerequisites:
            out_graph[courses[1]].append(courses[0])
            in_degrees[courses[0]] += 1
        while (len(visited) < numCourses):
            next_nodes = [i for i in in_degrees if in_degrees[i] == 0]
            if (len(next_nodes) <= 0):
                return []
            next_node = next_nodes[0]
            visited.add(next_node)
            for i in out_graph[next_node]:
                in_degrees[i] -= 1
            del(out_graph[next_node])
            del(in_degrees[next_node])
            path.append(next_node)
        return path
```

