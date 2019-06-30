---
title: Leetcode 70.爬楼梯
comments: true
toc: true
date: 2019-06-19 22:44:22
categories: Algorithm
tags: Other Algorithm
---

# 题目

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

**示例1:**
```java
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
```

**示例2:**
```java
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

# 思路

递归或者动态规划。

第n阶楼梯的走法 = 第n-1阶楼梯走法 + 第n阶楼梯走法。

边界条件，n<=3。

# 代码

```java
// 动态规划
class Solution {
    public int climbStairs(int n) {
        if(n<=3) return n;
        int[] dp = new int[n];
        for(int i=0;i<3;i++){
            dp[i] = i+1;
        }
        for(int i=3; i<n; i++){
            dp[i]=dp[i-2]+dp[i-1];
        }
        return dp[n-1];
    }
}
```

```java
// 递归
class Solution {
    public int climbStairs(int n) {
        if(n<=3) return n;
        return climbStairs(n-2) + climbStairs(n-1);
    }
}
```