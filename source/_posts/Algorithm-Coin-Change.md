---
title: Algorithm--Coin Change
toc: true
date: 2019-05-19 21:11:54
categories: Algorithm
tags: Dynamic Programming
---

# **Dynamic Programming**

## **Coin Change**

**Title Detail**

>You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

>**Example 1:**
```Java
Input: coins = [1, 2, 5], amount = 11
Output: 3 
Explanation: 11 = 5 + 5 + 1
```
>**Example 1:**
```Java
Input: coins = [2], amount = 3
Output: -1
```
>**Note:**

>You may assume that you have an infinite number of each kind of coin.

## 思路
[动态规划](https://zh.wikipedia.org/zh-hans/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92) 问题。

1. 用一个 `amount+1`长度的数组 `bp` 维护不同面值金额需要最小的硬币数。

    >`bp[0]=0`

    >`bp[1~amount]=amount+1`

    位置`0`表示面值为零，最小需要0个硬币，初始为0.

    位置`0~amount`假设需要amount个硬币，因硬币类型为`int`，即代表**无解**。
2. 对于硬币类型`coins[i]`，寻找`j=coins[i] ~j=amount`对应面值金额的最小硬币数，即为`min(dp[j], dp[j-coins[i]]+1)`。`dp[j-coins[i]]+1`代表放入一个该类型硬币。

    **前提：** `dp[j-coins[i]]`面值的金额不是无解，因为如果无解，那么`dp[j-coins[i]]+1`对于当前面值金额也是无解。

3. 返回`dp`数组第`amount`位置数字，即为所求。

## Algorithm
```Java
class Solution {
   public int coinChange(int[] coins, int amount) {
       if(coins.length==0) return -1;
       // 创建维护结果的数组
       int[] dp=new int[amount+1];
       Arrays.fill(dp,1,dp.length,amount+1);
       for(int i=0; i<coins.length;i++){
           // 前提
           for(int j=coins[i]; j<=amount;j++){
               // 硬币数量统计
               if(dp[j-coins[i]]!=amount+1){
                  dp[j]=Math.min(dp[j], dp[j-coins[i]]+1); 
               }
           }
       }
       if(dp[amount]!=amount+1) return dp[amount];
       return -1;
    }
}
```

[题目链接](https://leetcode-cn.com/problems/coin-change/)


