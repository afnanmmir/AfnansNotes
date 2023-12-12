---
title: "Pacific Atlantic Water Flow"
date: 2022-07-24
tags:
- software
- graphs
enableToc: true
---
The problem can be found [here](https://leetcode.com/problems/pacific-atlantic-water-flow/)

# The Problem
We are given an `m x n` rectangular island that borders both the Pacific and Atlantic Ocean. The Pacific Ocean touches the left and top edge of the island, and the Atlantic Ocean touches the right and bottom edges.

The island is split into `m*n` cells, and each cell has a given height above sea level: `heights[i][j]`.

If there is rain on a specific cell, the rain water can flow to the cell directly to the north, south, east, or west of the cell if and only if the height of the neighboring cell is less than or equal to the current cell's height. If a cell is adjacent to an ocean, then the water will flow into the ocean.

The goal is to return a list of coordinates such that if rain fell on that cell, the rain water can flow into **both** the Atlantic and Pacific Oceans.

# The Approach
If you have seen the post on the Max Area of an Island, a similar approach seems to make sense in this problem. We can represent the island as a graph. This graph will be directional. Each cell in the island will be a node. If that cell has a vertically or horizontally adjacent node with a height less than or equal to it, then there will be an edge from the cell to its adjacent node in that direction. It seems like, just like in Max Ara of an Island, we will be able to go to each cell and perform a graph traversal, and if we can reach a cell that borders the pacific ocean and a cell that borders the atlantic ocean, we can add this cell to the list of cells that can flow to both oceans. However, this can end up being very inefficient. We have to go through each cell, and in the worst case, we can reach every cell from each cell, so we would have a time complexity of $O(n^2 * m ^2)$, which is of degree 4. How can we make this more efficient.

Instead, let's work backwards. We know that in order for a cell to reach the Pacific Ocean, it must be able to reach a cell in the first column or the first row. Similarly, for a cell to reach the Atlantic Ocean, it must be able to reach a cell in the last row or the last column. We can reverse the edges on the graph so that the edges flow from lower elevation to higher elevation. We then can perform a graph traversal from all cells in the first row and column. All the cells that we visit can reach the Pacific Ocean in the original graph. Similarly, we perform a graph traversal starting from all cells in the last row and last column to find all the cells that can reach the Atlantic Ocean. After finding all cells that can reach the Pacific and Atlantic Ocean respectively, we return the intersection of these two sets, as these will be the cells that can reach both. Because we only do the graph traversal on $2n + 2m$ cells, we reduce our time complexity to $O((n+m)*n*m)$.

# Code
```py
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        ret = []
        rows = len(heights)
        cols = len(heights[0])
        pac_visits = [[0 for i in range(len(heights[0]))] for j in range(len(heights))]
        atl_visits = [[0 for i in range(len(heights[0]))] for j in range(len(heights))]
        def bfs(row, col, visits, heights):
            directions = [[0,1],[0,-1],[1,0],[-1,0]]
            visited = set()
            queue = collections.deque()
            queue.appendleft((row,col))
            while(len(queue) > 0):
                (row, col) = queue.popleft()
                if((row,col) not in visited):
                    visited.add((row, col))
                    visits[row][col] = 1
                    for i in directions:
                        newRow = row + i[0]
                        newCol = col + i[1]
                        if(newRow >= 0 and newRow < rows and newCol >= 0 and newCol < cols and heights[newRow][newCol] >= heights[row][col]):
                            queue.append((newRow, newCol))
        for i in range(len(heights)):
            bfs(i, 0, pac_visits, heights)
        for i in range(len(heights[0])):
            bfs(0, i, pac_visits, heights)
        for i in range(len(heights)):
            bfs(i, cols - 1, atl_visits, heights)
        for i in range(len(heights[0])):
            bfs(rows - 1, i, atl_visits, heights)
        for i in range(rows):
            for j in range(cols):
                if(pac_visits[i][j] == 1 and atl_visits[i][j] == 1):
                    ret.append([i,j])
        return ret
```
