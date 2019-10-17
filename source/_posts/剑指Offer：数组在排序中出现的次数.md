---
title: 剑指Offer：数组在排序中出现的次数
comments: true
toc: true
date: 2019-10-17 21:44:11
categories: Algorithm
tags: 剑指Offer
---

# 题目

统计一个数字在排序数组中出现的次数。

# 思路 + 代码

不能遍历，否则时间超出。二分查找，寻找边界位置。

```java
public class Solution {
    public int GetNumberOfK(int [] array , int k) {
       if(array==null||array.length==0)
           return 0;
        int l = binarySearch(array, k);
        int h = binarySearch(array, k+1);
        return (l>array.length-1 || array[l]!=k)?0:h-l;
    }
    private int binarySearch(int[] n, int k){
        int l = 0, r = n.length;
        int m;
        while(l<r){
            m = (r+l)/2;
            if(n[m]>=k)
            // 保证最后的位置等于或小于查找的元素
                r=m;
            else
                l=m+1;
        }
        return l;
    }
}
```
