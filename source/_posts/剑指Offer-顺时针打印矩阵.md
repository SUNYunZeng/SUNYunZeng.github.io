---
title: '剑指Offer: 顺时针打印矩阵'
comments: true
toc: true
date: 2019-09-30 20:14:36
categories: 算法题
tags: 剑指Offer
---

# 题目

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

# 思路 + 代码

制造一个子函数，用来打印每一层。函数接收的参数是需要打印的**行数跟列数**，以及起始打印的位置（都是 n * n）。

特殊情况处理：**打印行列错误、数组为空、单行或单列数组**

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printMatrix(int [][] matrix) {
       ArrayList<Integer> res = new ArrayList<>();
       printArr(matrix.length, matrix[0].length, 0, matrix, res);
       return res;
    }
    private void printArr(int I,int J,int s,int[][] arr, ArrayList<Integer> res){
        if(I<=0 || J<=0 || arr==null || arr.length==0 || arr[0].length==0)
            return;
        if(I==1){
            for(int i=0; i<J;i++){
                res.add(arr[s][s+i]);
            }
            return;
        }
        if(J==1){
            for(int i=0; i<I;i++){
                res.add(arr[s+i][s]);
            }
            return;
        }
        for(int i=0; i<J;i++){
            res.add(arr[s][s+i]);
        }
        for(int i=1;i<I;i++){
            res.add(arr[s+i][s+J-1]);
        }
        for(int i=J-1; i>0; i--){
            res.add(arr[s+I-1][s+i-1]);
        }
        for(int i=I-1; i>1; i--){
            res.add(arr[s+i-1][s]);
        }
        printArr(I-2,J-2,s+1,arr, res);
        return;
    }
}
```
