---
title: 剑指Offer：表示数值的字符串
comments: true
toc: true
date: 2019-10-25 21:57:44
categories: 算法题
tags: 剑指Offer
---

# 题目

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

# 思路 + 代码

**方法1: 正则匹配**
```java
/*
[]  ： 字符集合
()  ： 分组
?   ： 重复 0 ~ 1 次
+   ： 重复 1 ~ n 次
*   ： 重复 0 ~ n 次
.   ： 任意字符
\\. ： 转义后的 .
\\d ： 数字
*/

public class Solution {
    public boolean isNumeric(char[] str) {
        if(str == null || str.length == 0)
            return false;
        return new String(str).matches("[+-]?\\d*(\\.\\d+)?([Ee][+-]?\\d+)?");
    }
}
```

**方法2: 特殊情况排除**

```java
public class Solution {
    public boolean isNumeric(char[] str) {
        if(str == null || str.length == 0)
            return false;
        boolean hasE = false;
        boolean hasP = false;
        for(int i=0; i<str.length; i++){
            // 首字母出现的正负号合法
            if(i==0 && (str[i]=='+' || str[i]=='-'))
                continue;
            // 小数点判定（只出现一次，且不能在E（e）后面出现）
            if(str[i]=='.'){
                if(hasE || hasP)
                    return false;
                hasP = true;
            }
            // 指数判定，只出现一次，且不能在最后出现
            else if(str[i]=='E' || str[i]=='e'){
                if(i==str.length-1 || hasE)
                    return false;
                hasE = true;
            }
            // 第二次出现只能在指数后面
            else if(str[i]=='+' || str[i]=='-'){
                if(str[i-1]!='E' && str[i-1]!='e')
                    return false;
            }
            // 非法字符判定
            else if(!isInteger(str[i]))
                return false;
        }
        return true;
    }
    private boolean isInteger(char c){
        return c >='0' && c <='9';
    }
}
```