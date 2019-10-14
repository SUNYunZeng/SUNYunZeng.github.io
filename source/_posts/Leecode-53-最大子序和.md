---
title: Leetcode 53.最大子序和
comments: true
toc: true
date: 2019-06-17 17:18:17
categories: Algorithm
tags: 
    - Dynamic Programming 
    - 剑指Offer
---

# 题目

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**示例：**

```java
输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

# 思路

动态规划。

用一个数组维护一个包含当前数字的最大子序列，取该数组的最大值即为结果。


# 代码

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int len = nums.length;
        int result = nums[0];
        int[] dp = new int[len];
        dp[0] = nums[0];
        for(int i=1; i<len; i++){
            dp[i] = Math.max(dp[i-1]+nums[i], nums[i]);
            result = Math.max(dp[i], result);
        }
        return result;
    }
}
```

[题目链接](https://leetcode-cn.com/problems/maximum-subarray/)
