---
title: 剑指Offer：机器人的运动范围
comments: true
toc: true
date: 2019-10-31 19:59:05
categories: Algorithm
tags: Other Algorithm
---

# 题目

地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

# 思路 + 代码

**回溯方法**，类似[题目1](http://sunyunzeng.com/%E7%9F%A9%E9%98%B5%E4%B8%AD%E7%9A%84%E8%B7%AF%E5%BE%84/)、[题目2](http://sunyunzeng.com/Leetcode-%E5%B2%9B%E5%B1%BF%E6%9C%80%E5%A4%A7%E7%9A%84%E9%9D%A2%E7%A7%AF/)

```java
public class Solution {
    private int k;
    private int res;
    private int[][] next = new int[][]{{0,1},{0,-1},{1,0},{-1,0}};
    public int movingCount(int threshold, int rows, int cols)
    {
        if(rows<=0 || cols<=0 || threshold<0)
            return 0;
        k = threshold;
        res = 0;
        boolean[][] visit = new boolean[rows][cols];
        dfs(0,0, visit);
        return res;
    }
    private void dfs(int i, int j, boolean[][] visit){
        if(i<0 || j<0 || i>=visit.length || j>=visit[0].length || k<calNum(i)+calNum(j) || visit[i][j] ){
            return;
        }
        res++;
        visit[i][j] = true;
        for(int[] step: next){
            dfs(i+step[0], j+step[1], visit);
        }
        return;
    }
    private int calNum(int num){
        int res = 0;
        while(num > 0){
            res += num%10;
            num /= 10;
        }
        return res;
    }
}
```

