---
title: 剑指Offer：数组中只出现一次的数字
comments: true
toc: true
date: 2019-10-21 19:09:12
categories: Algorithm
tags: Other Algorithm
---

# 题目

一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

# 思路 + 代码

## 思路1

利用Map统计每个数字出现的次数。

```java
//num1,num2分别为长度为1的数组。传出参数
//将num1[0],num2[0]设置为返回结果
import java.util.*;
public class Solution {
    public void FindNumsAppearOnce(int [] array,int num1[] , int num2[]) {
        ArrayList<Integer> list = new ArrayList<Integer>();
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for(int num: array){
            if(!map.containsKey(num))
                map.put(num, 1);
            else
                map.put(num, map.get(num)+1);
        }
        for(Map.Entry<Integer, Integer> entry: map.entrySet()){
            if(entry.getValue()==1)
                list.add(entry.getKey());
        }
        num1[0] = list.get(0);
        num2[0] = list.get(1);
    }
}
```

## 思路2

与题目[只出现一次的数字](http://sunyunzeng.com/%E7%AE%97%E6%B3%95%E9%A2%98-%E4%BD%8D%E8%BF%90%E7%AE%97%E7%9B%B8%E5%85%B3/)类似，利用**异或**的知识。

唯一不同是这次是找出两个只出现一次的数字，所以需要找到这两个不同数字的区别，然后分别异或。

```java
//num1,num2分别为长度为1的数组。传出参数
//将num1[0],num2[0]设置为返回结果
import java.util.*;
public class Solution {
    public void FindNumsAppearOnce(int [] array,int num1[] , int num2[]) {
        int dif=0;
        for(int num: array)
            dif ^= num;
        // dif=1100 -dif=0100, dif &= -dif  dif=0100,即只保存最右侧异或结果不同的那一位，分割数组
        dif &= -dif;
        for(int num: array){
            if((num & dif)==0)
                num1[0] ^= num;
            else
                num2[0] ^= num;
        }
    }
}
```

[类似题目](http://sunyunzeng.com/%E5%89%91%E6%8C%87Offer%EF%BC%9A%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%8F%AA%E5%87%BA%E7%8E%B0%E4%B8%80%E6%AC%A1%E7%9A%84%E5%AD%97%E7%AC%A6/)