---
title: Leetcode 647.回文子串
comments: true
toc: true
date: 2019-07-28 08:21:05
categories: 算法题
tags: 动态规划
---

# 题目

给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被计为是不同的子串。

**示例1：**
```java
输入: "abc"
输出: 3
解释: 三个回文子串: "a", "b", "c".
```

**示例2：**
```java
输入: "aaa"
输出: 6
说明: 6个回文子串: "a", "a", "a", "aa", "aa", "aaa".
```

**注意：**
```java
输入的字符串长度不会超过1000。
```

# 思路 + 代码

回文子串的内部一定是回文子串，因此关键在于重复利用回文子串已经统计过的数值。

可采用双指针由回文子串向两端同时检测的方法。

分为偶数回文子串与奇数回文子串。

```java
class Solution {
    private int sum = 0;  
    public int countSubstrings(String s) {
        int len = s.length();
        for(int i=0; i<len; i++){
            count(s, i, i); // 统计奇数的回文子串数量
            count(s, i, i+1); //统计偶数的回文子串数量
        }
        return sum;
    }
    private void count(String s, int start, int end){
        while(start>=0 && end<s.length() && s.charAt(start)==s.charAt(end)){
            sum++;
            start--;
            end++;
        }
    }
}
```

# 思路 + 代码

采用动态规划，用一个二维数组 `dp[i][j]` 统计字符串从 i ~ j 是否是回文子串。

```java
class Solution {
    public int countSubstrings(String s) {
        int sum = 0;
        int length = s.length();
        // 用dp数组存储回文子串结果，第一维为长度（有哨兵），第二维为起始位置
        boolean[][] dp = new boolean[length+1][length];
        for(int i=1; i<length+1; ++i){
            for(int j=0; j<length; ++j){
                // 所有长度为1的子串都为回文子串
                if(i==1){
                    dp[j][j] = true;
                    sum++;
                }
                // 所有长度为2的子串情况
                else if(i==2){
                    if(j+1<length && s.charAt(j)==s.charAt(j+1)){
                        dp[j][j+1] = true;
                        sum++;
                    }                                        
                }
                // 长度大于3的情况
                else{
                    if(j+i-1<length && dp[j+1][j+i-2] && s.charAt(j)==s.charAt(j+i-1)){
                        dp[j][j+i-1] = true;
                        sum++;
                    }
                }
            }
        }
        return sum;
    }
}
```