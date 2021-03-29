---
title: Minimum-Falling-Path-Sum
toc: true
date: 2019-05-25 21:11:46
categories: 算法题
tags: 动态规划
---

# 下降路径最小和

**题目细节**

>给定一个方形整数数组 A，我们想要得到通过 A 的下降路径的最小和。
下降路径可以从第一行中的任何元素开始，并从每一行中选择一个元素。在下一行选择的元素和当前行所选元素最多相隔一列。

**示例1**
```Java
输入：[[1,2,3],[4,5,6],[7,8,9]]
输出：12

```
>**解释：**
>可能的下降路径有：
    ```Java
    [1,4,7], [1,4,8], [1,5,7], [1,5,8], [1,5,9]
    [2,4,7], [2,4,8], [2,5,7], [2,5,8], [2,5,9], [2,6,8], [2,6,9]
    [3,5,7], [3,5,8], [3,5,9], [3,6,8], [3,6,9]
    ```
>和最小的下降路径是 **[1,4,7]**，所以答案是 **12**。

**提示**
```Java
1 <= A.length == A[0].length <= 100
-100 <= A[i][j] <= 100
```

## 思路

典型的二维动态数组题目。创建一个二维的数组**A[row][column]**存储结果，每一个位置存储的是第一行到该位置最小的下降路径。

**一般情况**
```Java
A[i][j] +=min(min(A[i-1][j-1],A[i-1][j]),A[i-1][j+1]);
```

**前后两列情况**
```Java
第一列: A[i][j] += min(A[i-1][j],A[i-1][j+1]);
最后一列: A[i][j] += min(A[i-1][j-1],A[i-1][j]);
```

## Algorithm
```Java
class Solution {
    public int minFallingPathSum(int[][] A) {
        int row = A.length;
        int col = A[0].length;
        if(row==1){
            return A[0][0];
        }
        for(int i=1;i<row;++i){
            for(int j=0;j<col;++j){
                if(j-1<0){
                    A[i][j] += min(A[i-1][j],A[i-1][j+1]);
                }
                else if(j+1>col-1){
                    A[i][j] += min(A[i-1][j-1],A[i-1][j]);
                }
                else{
                    A[i][j] +=min(min(A[i-1][j-1],A[i-1][j]),A[i-1][j+1]);
                }
            }
            
        }
        int result = Integer.MAX_VALUE;
        for(int i=0;i<col;++i){
            if (A[row-1][i] < result) {
                result = A[row-1][i];
            }
        }
        return result;
    }
    public int min(int a, int b){
        return (a<=b?a:b);
    }
}
```
[题目链接](https://leetcode-cn.com/problems/minimum-falling-path-sum/submissions/)

[类似题目](https://sunyunzeng.github.io/SUNYunZeng.github.io/2019/05/21/Algorithm-Minimum-path-sum/)
