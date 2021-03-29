---
title: 剑指Offer：数组中的逆序对
comments: true
toc: true
date: 2019-10-17 20:46:53
categories: 算法题
tags: 剑指Offer
---

# 题目

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007

**题目描述:**

```java
题目保证输入的数组中没有的相同的数字

数据范围：

	对于%50的数据,size<=10^4

	对于%75的数据,size<=10^5

	对于%100的数据,size<=2*10^5
```

**示例1**
```java
输入
1,2,3,4,5,6,7,0

输出
7
```

# 思路

归并排序，先拆分成单个的数字，然后向上归并同时统计逆数数量。

```java
public class Solution {
    private int count = 0;
    public int InversePairs(int [] array) {
        if(array==null || array.length==0)
            return -1;
        mergeSort(array, 0, array.length-1);
        return count;
    }
    private void mergeSort(int[] nums, int left, int right){
        if(left>=right)
            return;
        int l = left, r = right, m = (left + right)/2;
        mergeSort(nums, l, m);
        mergeSort(nums, m+1, r);
        merge(nums, l, m, r);
    }
    private void merge(int[] nums, int left, int mid, int right){
        int len = right-left;
        int[] tmp = new int[len+1];
        int l = mid, r = right;
        while(l >= left && r >= mid+1){
            if(nums[l]>nums[r]){
                count += (r-mid);
                tmp[len--] = nums[l--];
                if(count >= 1000000007)
                    count %= 1000000007;
            }else{
                tmp[len--] = nums[r--];
            }
        }
        // 上述判定并不能概括所有的情况，因此用两个循环继续完成辅助数组tmp
        while(l >= left)
            tmp[len--] = nums[l--];
        while(r >= mid+1)
            tmp[len--] = nums[r--];
        // 排序
        for(int i=0; i<tmp.length; i++)
            nums[left+i] = tmp[i];
    }
}
```
