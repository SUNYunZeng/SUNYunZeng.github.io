---
title: Leetcode 674.最长连续子序列
comments: true
toc: true
date: 2019-09-23 22:38:36
categories: 算法题
tags: 动态规划
---

# 题目

给定一个未经排序的整数数组，找到最长且连续的的递增序列。

**示例1：**

```java
输入: [1,3,5,4,7]
输出: 3
解释: 最长连续递增序列是 [1,3,5], 长度为3。
尽管 [1,3,5,7] 也是升序的子序列, 但它不是连续的，因为5和7在原数组里被4隔开。 
```

**示例2：**

```java
输入: [2,2,2,2,2]
输出: 1
解释: 最长连续递增序列是 [2], 长度为1。
```

**注意：**数组长度不会超过10000。

# 思路 + 代码

## 思路1

我的思路，用一个栈进行递增子序列长度的检查，用<font color=#f07c82>res</font>缓存结果。

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        if(nums==null || nums.length==0)
            return 0;
        Stack<Integer> s = new Stack<Integer>();
        int res = 1;
        int len = nums.length;
        s.push(nums[0]);
        for(int i=1; i<len; i++){
            if(nums[i]<=nums[i-1]){
                res = Math.max(res, s.size());
                s.clear();
            }
            s.push(nums[i]);
        }
        res = Math.max(res, s.size());
        return res;
    }
}
```

# 思路2

动态规划，用一个长度为n的数值维度维护每个位置的最大长度，然后取其中最大值。

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        if(nums==null || nums.length==0)
            return 0;
        int res = 1;
        int len = nums.length;
        int[] dp = new int[len];
        Arrays.fill(dp, 1);
        for(int i=1; i<len; ++i){
            if(nums[i]>nums[i-1]){
               dp[i] += dp[i-1];
               res = Math.max(dp[i], res);
            }
        }
     
        return res;
    }
}
```
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
