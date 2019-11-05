---
title: LeetCode 22.括号生成
comments: true
toc: true
date: 2019-11-05 22:36:55
categories: Algorithm
tags: Other Algorithm
---

# 题目

给出 n 代表生成括号的对数，请你写出一个函数，使其能够生成所有可能的并且有效的括号组合。

例如，给出 n = 3，生成结果为：

```java
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

# 思路 + 代码

回溯法，通过两个整数统计“（”与“）”的数量。

```java
class Solution {
    private List<String> res = new ArrayList<>();
    public List<String> generateParenthesis(int n) {
        if(n<1)
            return res;
        backtracing("", 0, 0, n);
        return res;
    }
    private void backtracing(String s, int open, int close, int n){
        if(close>open || open>n || close>n)
            return;
        if(s.length()==2*n){
            res.add(s);
            return;
        }
        String str = new String(s);
        backtracing(str+"(", open+1, close, n);
        backtracing(str+")", open, close+1, n);
    }
}
```
