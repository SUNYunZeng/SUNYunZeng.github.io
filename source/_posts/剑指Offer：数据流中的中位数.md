---
title: 剑指Offer：数据流中的中位数
comments: true
toc: true
date: 2019-10-31 09:37:29
categories: 算法题
tags: 剑指Offer
---

# 题目

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。我们使用Insert()方法读取数据流，使用GetMedian()方法获取当前读取数据的中位数。

# 思路 + 代码

参考[CyC](https://github.com/CyC2018/CS-Notes/blob/master/notes/%E5%89%91%E6%8C%87%20Offer%20%E9%A2%98%E8%A7%A3%20-%2040~49.md#411-%E6%95%B0%E6%8D%AE%E6%B5%81%E4%B8%AD%E7%9A%84%E4%B8%AD%E4%BD%8D%E6%95%B0)

```java
import java.util.*;
public class Solution {
    // left-最大堆  right-最小堆
    private PriorityQueue<Integer> left = new PriorityQueue<Integer>((o1, o2)->o2-o1);
    private PriorityQueue<Integer> right = new PriorityQueue<Integer>();
    private int N = 0;
    public void Insert(Integer num) {
        if(N%2==0){
            left.add(num);
            right.add(left.poll());
        }else{
            right.add(num);
            left.add(right.poll());
        }
        // N是滞后计数，表示一数据流的长度是多少
        N++;
    }

    public Double GetMedian() {
        if(N%2==0){
            return (left.peek()+right.peek())/2.0;
        }else{
            return (double)right.peek();
        }
    }
```
