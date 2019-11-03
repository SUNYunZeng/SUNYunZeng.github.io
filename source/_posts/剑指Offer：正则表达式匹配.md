---
title: 剑指Offer：正则表达式匹配
comments: true
toc: true
date: 2019-10-25 20:43:14
categories: Algorithm
tags: 剑指Offer
---

# 题目

请实现一个函数用来匹配包括'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配。

# 思路 + 代码

**动态规划：**利用一个dp(x, y)的数组表示原字符串 s[0, x) 与匹配字符串 p[0, y)是否匹配。

**状态转移：**

对于dp(x, y)：

1. 如果 p(y) == '.' || p(y) == s(x-1), dp(x, y)
 = dp(x-1, y-1)。

2. 如果 p(y) == '*'

    2.1 如果 p(y-1) == s(x) || p(y-1) == '.'
    
    1) '*' 复制多个：dp(x, y) = dp(x-1, y)
    2) '*' 复制一个：dp(x, y) = dp(x, j-1)
    3) '*' 复制零个：dp(x, y) = dp(x, j-2)

    2.2 如果 s 为空且不满足 2.1，则 '*' 复制零个：dp(x, y) = dp(x, j-2)

```java
public class Solution {
    public boolean match(char[] str, char[] pattern)
    {
        int m = str.length+1;
        int n = pattern.length+1;
        boolean[][] dp = new boolean[m][n];
        dp[0][0] = true;
        // 空字符串处理
        for(int i=1; i<n; i++)
            if(pattern[i-1]=='*')
                dp[0][i] = dp[0][i-2];
        for(int i=1; i<m; i++){
            for(int j=1; j<n; j++){
                if(str[i-1]==pattern[j-1] || pattern[j-1]=='.')
                    dp[i][j] = dp[i-1][j-1];
                else if(pattern[j-1]=='*'){
                    if(pattern[j-2]==str[i-1] || pattern[j-2]=='.'){
                        dp[i][j] |= dp[i-1][j]; // 复制多个
                        dp[i][j] |= dp[i][j-1]; //复制一个
                        dp[i][j] |= dp[i][j-2]; // 删除一个
                    }else
                        dp[i][j] |= dp[i][j-2]; // 删除一个
                }
            }
        }
        return dp[m-1][n-1];
    }
}
```
