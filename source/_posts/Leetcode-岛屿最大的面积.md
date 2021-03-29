---
title: Leetcode 岛屿最大的面积
comments: true
toc: true
date: 2019-09-07 09:46:27
categories: 算法题
tags: 回溯法
---

# 题目

给定一个包含了一些 0 和 1的非空二维数组 grid , 一个 岛屿 是由四个方向 (水平或垂直) 的 1 (代表土地) 构成的组合。你可以假设二维矩阵的四个边缘都被水包围着。

找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为0。)

**示例1:**
```java
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
```

对于上面这个给定矩阵应返回 6。注意答案不应该是11，因为岛屿只能包含水平或垂直的四个方向的‘1’。

**示例2:**
```java
[[0,0,0,0,0,0,0,0]]
```

对于上面这个给定的矩阵, 返回 0。

**注意:** 给定的矩阵grid 的长度和宽度都不超过 50。

# 思路+代码

一开始想的是动态规划，但是该题没有规律的求解方法，即岛屿的形状无法用统一的方法计算。

此题采用 <font color=#f07c82>DFS（深度优先遍历）</font>的方法求解。

DFS一般解体模板（<u>https://blog.csdn.net/weixin_43272781/article/details/82959089</u>）。

```java
int check(参数)
{
    if(满足条件)
        return 1;
    return 0;
}
 
void dfs(int step)
{
        判断边界
        {
            相应操作
        }
        尝试每一种可能
        {
               满足check条件
               标记
               继续下一步dfs(step+1)
               恢复初始状态（回溯的时候要用到）
        }
}   
```

此题的解法：

+ 边界条件是二维矩阵的边界。

+ 尝试每一种可能是上、下、左、右四个方向进行遍历。

+ check是是否为陆地（数值是否为1）。

+ 标记是将遍历过的陆地变成海洋（置为0）。

```java
class Solution {
    private int row;
    private int col;
    public int maxAreaOfIsland(int[][] grid) {
        row = grid.length;
        col = grid[0].length;
        if(row==0 || col==0)
            return 0;
        int res = 0;
        for(int i=0; i<row; i++){
            for(int j=0; j<col; j++){
                if(grid[i][j]>0){
                    res = Math.max(res, dfs(grid, i, j));   
                }
            }
        } 
        return res;
    }
    private int dfs(int[][] g, int i, int j){
        int sum = 1, x=0, y=0;
        int[][] dir = new int[][]{{1,0},{-1,0},{0,1},{0,-1}};
        //避免再次被选到
        g[i][j] = 0;
        for(int s=0;s<4;s++){
            x = i + dir[s][0];
            y = j + dir[s][1];
            if(x>=0 && x<row && y>=0 && y<col && g[x][y]>0){
                sum += dfs(g, x ,y);
            }
        }
        return sum;
    }
}
```

题目链接：<u>https://leetcode-cn.com/explore/interview/card/bytedance/243/array-and-sorting/1034/</u>