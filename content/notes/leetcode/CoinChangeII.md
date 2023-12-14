---
title: "Coin Change II"
date: 2022-07-24
lastmod: 2022-07-24
tags:
- leetcode
- Dynamic Programming
---
The problem can be found [here](https://leetcode.com/problems/coin-change-2/)

# The Problem
Given an array of coin denominations `coins` and an integer `amount`, return the number of combinations of coins you can make that add up to `amount`. If the amount cannot be made by denominations given, then return `0`.

Example:
```
coins: [1,2,5]
amount: 5
output: 4
explanation
1 + 1 + 1 + 1 + 1 = 5
2 + 2 + 1 = 5
1 + 1 + 1 + 2 = 5
5 = 5
```

# The Approach
We take a dynamic programming approach to this problem, however, unlike coin change I, we cannot solve this problem with just 1 dimension. If we go down this route, we will see that it becomes flawed early on. 
In 1 dimension, we would have $OPT(i)$, where $i$ is the value we are at. We would then iterate through each coin denomination and return $\sum_{j = 0}^{n} OPT(i - coins[j])$. If this is used on the example case, we see how it is flawed. The base case is when $i = 0$, where there is 1 way to make it. We then go to $i = 1$. Again, this would return 1, as there is one way to reach $1 - 1$, and every other coin denomination leads to a negative number. For $i = 2$, there is 1 way to reach 1, and 1 way to reach 0, so $OPT(2) = 2$.  For $i = 3$, however, we see that there are 2 ways to reach 2, and 1 way to reach 1, so we would return $OPT(3) = 3$, which is wrong.

Therefore, we need 2 dimensions, with the second dimension being the amount of coin denominations we are allowed to use. This gives us an `amount + 1 x coins.length + 1` dimension table.

Our base case would be as follows: given that we cannot use any coin denominations, there is no way to reach get any amount except 0. ($OPT(i,0) = 0$). Additionally, given any number of coin denominations, there will always be 1 way to get an amount of 0. ($OPT(0,i)$ = 1). Now, how do we calculate $OPT(i,j)$, for any given $i$ and $j$. If the goal is to find the amount of ways to create amount i with up to j coin denominations, it would make sense that it would at least include all the ways to create i with $j-1$ denominations. In addition to this, we add the amount of ways we can create $i - coins[j]$ with j coin denominations because we are simulating adding one more of the $j^{th}$ coin denomination. Thus, we have $OPT(i,j) = OPT(i, j - 1) + OPT(i - \text{coins[j]},j)$. We then return the value where $\text{i = amount}$ and $\text{j = coins.length}$. This gives us enough to write the code.

# Code
``` java
class Solution {
    public int change(int amount, int[] coins) {
        int[][] dp = new int[coins.length + 1][amount + 1];
        for(int i = 0;i<dp[0].length;i++){
            dp[0][i] = 0;
        }
        for(int i = 0;i < dp.length;i++){
            dp[i][0] = 1;
        }
        for(int i = 1;i < dp[0].length;i++){
            for(int j = 1;j < dp.length;j++){
                dp[j][i] = dp[j-1][i];
                if(i - coins[j-1] >= 0){
                    dp[j][i] += dp[j][i - coins[j-1]];
                }
            }
        }
        return dp[coins.length][amount];
    }
}
```

