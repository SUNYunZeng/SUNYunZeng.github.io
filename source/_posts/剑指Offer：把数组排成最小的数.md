---
title: 剑指Offer：把数组排成最小的数
comments: true
toc: true
date: 2019-10-14 21:23:50
categories: Algorithm
tags: 剑指Offer
---

# 题目

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

# 思路 + 代码

**字符串的排序。**

```java
import java.util.*;

public class Solution {
    public String PrintMinNumber(int [] numbers) {
        if(numbers==null || numbers.length==0){
            return "";
        }
        int len = numbers.length;
        String[] strs = new String[len];
        for(int i=0; i<len; i++)
            strs[i] = numbers[i]+"";
        Arrays.sort(strs, (a, b)->(a+b).compareTo(b+a));
        StringBuilder sb = new StringBuilder();
        for(String s: strs)
            sb.append(s);
        return sb.toString();
    }
}
```