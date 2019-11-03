---
title: 剑指Offer：剪绳子
comments: true
toc: true
date: 2019-10-31 20:25:30
categories: Algorithm
tags: 剑指Offer
---

# 题目

给你一根长度为n的绳子，请把绳子剪成m段（m、n都是整数，n>1并且m>1），每段绳子的长度记为k[0],k[1],...,k[m]。请问k[0]xk[1]x...xk[m]可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

（2 <= n <= 60）

# 思路 + 代码

**动态规划**

可以分解子问题，即剪一刀，产生两段绳子，长度分别是 i 与 n-i, 在这两条绳子上可以继续剪：

```java
f(n) = max(f(i), f(n-i))
```

```java
public class Solution {
    public int cutRope(int target) {
        int[] dp = new int[target+1];
        dp[1] = 1;
        for(int i=2; i<=target; i++){
            for(int j=1; j<i; j++){
                // 剪一刀长度为j，则产生长度为j与i-j长度的绳子
                // 进而判断长度为j的绳子的大值，为max(j, dp[j]);
                // 最后去dp[j]的最大值
                dp[i] = Math.max(dp[i], Math.max(j*(i-j), dp[j]*(i-j)));
            }
        }
        return dp[target];
    }
}
```
