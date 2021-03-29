---
title: Leetcode 最长公共前缀 (String 练习 01)
comments: true
toc: true
date: 2019-08-31 17:03:25
categories: 算法题
tags: LeetCode
---

# 题目

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

**示例1:**
```java
输入: ["flower","flow","flight"]
输出: "fl"
```

**示例2:**
```java
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```

**说明:**
```java
所有输入只包含小写字母 a-z 。
```

# 思路1

暴力法，时间复杂度O(n^3)。

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        String ans = "";
        if(strs.length==0) return ans;
        lable:{
            for(int j=0; j<strs[0].length(); j++){
                char c = strs[0].charAt(j);
                for(int i=1; i<strs.length; i++){
                    if(j>=strs[i].length() || !isCharEqual(c, strs[i], j)){
                        break lable;
                    }
                }
                ans += c;
            }
        }
        return ans;
    }
    private boolean isCharEqual(char c, String s, int i){
        return c==s.charAt(i);
    }
}
```

# 思路2

巧用String提供的一些API，例如substring(),判定子串的位置。

```Java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        String ans;
        if(strs.length==0) return "";
        ans = strs[0];
        for(int i=1; i<strs.length; i++){
            while(strs[i].indexOf(ans)!=0){
                ans = ans.substring(0, ans.length()-1);
                if(ans.isEmpty())
                    return ans;
            }
        }
        return ans;
    } 
}
```