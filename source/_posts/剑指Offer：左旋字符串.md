---
title: 剑指Offer：左旋字符串
comments: true
toc: true
date: 2019-10-22 22:03:20
categories: 算法题
tags: 剑指Offer
---

# 题目

汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！

# 思路 + 代码

先将 "abc" 和 "XYZdef" 分别翻转，得到 "cbafedZYX"，然后再把整个字符串翻转得到 "XYZdefabc"。

```java
public class Solution {
    public String LeftRotateString(String str,int n) {
        if(str==null || str.length()==0)
            return str;
        char[] chs = str.toCharArray();
        int len = chs.length-1;
        inverse(chs, 0, n-1);
        inverse(chs, n ,len);
        inverse(chs, 0, len);
        return new String(chs);
    }
    private void inverse(char[] chs, int i, int j){
        while(i<j){
            swap(chs, i, j);
            i++;
            j--;
        }
    }
    private void swap(char[] chs, int i, int j){
        char t = chs[i];
        chs[i] = chs[j];
        chs[j] = t;
    }
}
```
