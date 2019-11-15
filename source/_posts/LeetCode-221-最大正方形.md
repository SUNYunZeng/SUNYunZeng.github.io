---
title: LeetCode 221.最大正方形
comments: true
toc: true
date: 2019-11-09 22:02:04
categories: Algorithm
tags: Dynamic Programming
---

# 题目

在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。

**示例：**

```java
输入: 

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

输出: 4
```

# 思路 + 代码

是动态规划的题目。

关键在于问题的转化。

问题转化为最长边长。

然后截止当前位置的最长边长是左边、上边及右上三者中的最小值 + 1。

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        if(matrix.length==0 || matrix[0].length==0)
            return 0;
        int row = matrix.length;
        int col = matrix[0].length;
        int[][] dp = new int[row+1][col+1];
        int max_side = 0;
        for(int i=1; i<=row; i++){
            for(int j=1; j<=col; j++){
                if(matrix[i-1][j-1]=='1'){
                    dp[i][j] = Math.min(Math.min(dp[i][j-1], dp[i-1][j]), dp[i-1][j-1])+1;
                    max_side = Math.max(max_side, dp[i][j]);
                }   
            }
        }
        return max_side*max_side;
    }
}
```


