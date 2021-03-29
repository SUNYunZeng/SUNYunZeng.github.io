---
title: Algorithm--Minimum path sum
toc: true
date: 2019-05-21 16:47:00
categories: 算法题
tags: 
    - 动态规划
---

# **Dynamic Programming**

## **Minimum Path Sum**

**Title Detail**

>Given an integer array A, you partition the array into (contiguous) subarrays of length at most K.  After partitioning, each subarray has their values changed to become the maximum value of that subarray.

>Return the largest sum of the given array after partitioning.

>**Example 1:**
```Java
Input: A = [1,15,7,9,2,5,10], K = 3
Output: 84
Explanation: A becomes [15,15,15,9,10,10,10]
```
>**Note:**
```Java
1 <= K <= A.length <= 500
0 <= A[i] <= 10^6
```

## 思路
[动态规划](https://zh.wikipedia.org/zh-hans/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92) 问题。

1. 用原来的`grid`矩阵存储路径和
2. 注意三种特殊情况，即矩阵**初始位置、顶栏及左侧栏**和求解。

    初始：`grid[0][0] = grid[0][0]`

    顶栏：`grid[i][j] = grid[i][j-1]`

    左侧栏：`grid[i][j] = grid[i-1][j]`
3. 其余位置：`grid[i][j] = min(grid[i][j-1], grid[i-1][j])`

## Algorithm

```Java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        for(int i=0; i<m;i++){
            for(int j=0; j<n; j++){
                if(i==0&&j==0){
                    continue;
                }
                else if(i==0&&j!=0){
                    grid[i][j] += grid[i][j-1]; 
                }
                else if(j==0&&i!=0){
                    grid[i][j] += grid[i-1][j];
                }else{
                    grid[i][j] += Math.min(grid[i-1][j], grid[i][j-1]);
                }
            }
        }
        return grid[m-1][n-1];
    }
}
```

[题目链接](https://leetcode-cn.com/problems/minimum-path-sum/submissions/)