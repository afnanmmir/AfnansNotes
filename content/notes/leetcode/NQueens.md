---
title: "Palindrome Partitioning"
date: 2026-01-06
lastmod: 2026-01-06
tags:
- leetcode
- backtracking
enableToc: true
---
The problem can be found [here](https://leetcode.com/problems/n-queens/description/)

# The Problem
The n-queens puzzle is the problem of placing `n` queens on an n x n chessboard such that no two queens attack each other.

Given an integer `n`, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space, respectively.

# The Approach
Considering we are trying to find all possible solutions/combinations for this problem, we can deduce that this is a backtracking problem. The decision that we have to make is for each cell, whether to put a queen in that cell, or to not put a queen in that cell. First, to reduce our search/decision space, because no two queens can be in the same row for this problem (because 2 queens in the same row can attack each other), once we add a queen to one row, the backtracking can automatically skip all the rest of the cells in that row, and we can move on to the next row.

This leads us to our backtracking approach. We will backtrack on the rows. The base case will be when the row we are on exceeds the board. When that happens, we have found a valid combination for N-queens, and we can add this combination to the list to return to. At each row, we would iteration through each of the columns in the row. At each column, we check if this is a valid cell to place a queen on. What is the condition that we need to satisfy to ensure that this is a valid cell? We have to ensure the following:
1. There is no other queen in that same column
2. There is no other queen on the same left to right diagonal
3. There is no other queen on the same right to left diagonal
We can do this by creating a set for each case, and when we add a queen, we add the column to the column set, we add the left to right diagonal to the l-to-r diagonal set, and add the right to left to the r-to-l diagonal set. How do we encode the left to right diagonal and the right to left diagonal. For the left to right diagonal, we can use the difference between the row and the column (row - col). All cells that have the same `row - col` value are in the same left to right diagonal, and all cells with the same `row + col` value are in the same right to left diagonal, so we can add these values to their respective sets.

When we want to add a queen to a cell, we check if its column, l-to-r diagonal, or r-to-l diagonal are in their respective sets. If they are not in any of the sets, we can add a queen there, we add all the values to their respective sets, and then we recurse to the next row. After recursing, we have to perform the cleanup after the recursion, removing the queen from the cell, and removing all the values from their respective sets.

This would complete the backtracking algorithm. The reason that this would only generate valid boards is _we only recurse if we find a valid cell to put a queen on_. This means that say we have `n = 4`, and we have put 2 queens on row 0 and row 1 that are valid. For row 3, let's say, we don't find any cell that is valid. In this case, we would never perform the recursion to the next row, and the function call will die performing another recursive call that would reach the base case. Because we will never reach the base case, we will never add this invalid combination to the return list.

# The Code
```py
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        rows_used = set()
        cols_used = set()
        l_to_r_diag = set()
        r_to_l_diag = set()
        ret_list = []
        board = [["."] * n for i in range(n)]
        def isValidPlacement(row, col):
            if (row in rows_used):
                return False
            if (col in cols_used):
                return False
            if (row - col in l_to_r_diag):
                return False
            if (row + col in r_to_l_diag):
                return False
            return True
        def backtrack(row):
            if (row >= n):
                ret_list.append(["".join(row_chars) for row_chars in board])
                return

            for i in range(n):
                if (isValidPlacement(row, i)):
                    board[row][i] = "Q"
                    rows_used.add(row)
                    cols_used.add(i)
                    l_to_r_diag.add(row - i)
                    r_to_l_diag.add(row + i)
                    backtrack(row + 1)
                    board[row][i] = "."
                    rows_used.remove(row)
                    cols_used.remove(i)
                    l_to_r_diag.remove(row - i)
                    r_to_l_diag.remove(row + i)
        backtrack(0)
        return ret_list

        
```