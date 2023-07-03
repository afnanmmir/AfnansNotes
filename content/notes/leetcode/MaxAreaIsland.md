---
title: "Max Area of Island"
tags:
- coding
- software
- graphs
enableToc: true
---
The problem can be found [here](https://leetcode.com/problems/max-area-of-island/)

# The Problem
We are given an `m x n` matrix of 1's and 0's, `grid`, such that the 1's represent land, and 0's represent water. An island is a group of 1's that are connected either horizontally or vertically. The goal is to determine the island in the grid with the most area.

# The Approach
Although the problem represents the islands and the water as a matrix, it would serve us better to represent this as a graph. We can represent every `1` in the matrix as a node in the graph, and two nodes are adjacent to each other (there is an edge between the two nodes) if and only if they are adjacent either horizontally or vertically in the grid.

For example:
```grid = [[1,0,0,0],[1,1,0,0],[0,0,1,0]]```
In this grid, there are four nodes, as there are four 1`s. Three of the nodes are connected because there is a path to and from each one of them. The 1 in row 2 is connected to no other node, as there is no other 1 adjacent to it.

If we think of the problem this way, we can perform graph traversals in order to determine the area of an island. We can perform either a breadth-first search or a depth-first search, and everytime we reach a node we have not visited, then we increase the area by one. Once the traversal is done, we will have our area, and we will be able to compare it with the maximum area we are keeping track of.

In the matrix, we will have potentially multiple graphs to go through, as there may be multiple islands. In order to make sure we traverse all graphs, we will perform a traversal of the grid, and perform a graph traversal everytime we encounter some land (i.e we encounter a 1). However, to make sure we don't accidentally traverse the same island multiple times, we will have a set that keeps track of all points we have visited so far.

In summary, we will traverse the whole matrix to find 1's. When we find a 1 that we have not already visited, we perform a graph traversal (in this case, a BFS) and keep track of how many 1's we encounter in the graph traversal. Additionally, we keep track of all the points we have visited in the traversal to make sure we do not visit that node again later. After the traversal is done, we update our maximum area accordingly if the area is larger. Once we have traversed all the islands, we will have our maximum area of an island.

# Code
```py
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        directions = [(1,0), (-1,0), (0,1), (0,-1)]
        visited = set()
        maxArea = 0
        rows = len(grid)
        cols = len(grid[0])
        def bfs(row, col, grid):
            queue = collections.deque()
            queue.appendleft((row,col))
            area = 0
            while(len(queue) > 0):
                (row, col) = queue.popleft()
                if((row,col) not in visited):
                    area += 1
                    # print(row, col, area)
                    visited.add((row,col))
                    for i in directions:
                        newRow = row + i[0]
                        newCol = col + i[1]
                        # if(row == 0 and col == 7):
                        #     # print(newRow, newCol)
                        if(newRow >= 0 and newRow < rows and newCol >= 0 and newCol < cols and grid[newRow][newCol] == 1):
                            # if(row == 0 and col == 7):
                            #     # print(newRow, newCol)
                            queue.append((newRow, newCol))
            return area
        for row in range(rows):
            for col in range(cols):
                if((row, col) not in visited):
                    # print("Hello")
                    if(grid[row][col] == 1):
                        # print("hello")
                        # print(row, col)
                        area = bfs(row, col, grid)
                        maxArea = max(maxArea, area)
        return maxArea