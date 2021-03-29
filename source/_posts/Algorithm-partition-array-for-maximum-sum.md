---
title: Algorithm--Partition-array-for-maximum-sum
toc: true
date: 2019-05-21 15:13:00
categories: 算法题
tags:
    - 动态规划 
---

# **Dynamic Programming**

## **Partition Array for Maximum Sum**

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

1. 用一个长度为`A.length`的数组`dp`维护最终的加和结果。

2. 计算思想如下：

    dp[i] := max sum of A[0] ~ A[i]

    dp[i] = max{dp[i – k] + max(A[i+1-k:i]) * k}, 1 <= k <= min(i, K)

3. 最终返回结果

    dp[A.length-1]

## Algorithm

```Java
class Solution {
    public int maxSumAfterPartitioning(int[] A, int K) {
        int len = A.length;
        int[] dp = new int[len];
        for(int i=0; i<A.length; ++i){
            int max_num = A[i];
            for(int k=1; k<=K && i+1-k>=0; ++k){
                max_num = Math.max(max_num, A[i+1-k]);
                dp[i] = Math.max(dp[i], (i-k>=0?dp[i-k]:0) + k*max_num);
            }
        }
        return dp[len-1];
    }
}
```

[题目链接](https://leetcode-cn.com/problems/partition-array-for-maximum-sum/)
