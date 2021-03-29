---
title: LeetCode 62.不同路径
comments: true
toc: true
date: 2019-11-08 21:36:50
categories: 算法题
tags: 动态规划
---

# 题目

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

问总共有多少条不同的路径？

# 思路 + 代码

动态规划。。用回溯竟然做不出来

```java
class Solution {
    public int uniquePaths(int m, int n) {
        if(m<=0 || n<=0)
            return 0;
        int[][] dp = new int [m][n];
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                if(i==0)
                    dp[i][j] = 1;
                else if(j==0)
                    dp[i][j] = 1;
                else
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
}
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/unique-paths
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
