---
title: 剑指Offer：滑动窗口的最大值
comments: true
toc: true
date: 2019-10-31 18:41:27
categories: Algorithm
tags: 剑指Offer
---

# 题目

给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。

# 思路 + 代码

用一个最大堆维护中间的判断结果，每次只需对顶元素调整即可。

```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> maxInWindows(int [] num, int size)
    {
        ArrayList<Integer> res = new ArrayList<Integer>();
        if(size<=0 || size>num.length)
            return res;
        // 最大堆维护结果
        PriorityQueue<Integer> queue = new PriorityQueue<Integer>((o1, o2)->o2-o1);
        for(int i=0; i<size; i++)
            queue.add(num[i]);
        res.add(queue.peek());
        for(int i=size; i<num.length; i++){
            queue.remove(num[i-size]);
            queue.add(num[i]);
            res.add(queue.peek());
        }
        return res;
    }
}
```
