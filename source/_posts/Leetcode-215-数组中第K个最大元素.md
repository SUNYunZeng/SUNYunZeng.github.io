---
title: Leetcode 215.数组中第K个最大元素
comments: true
toc: true
date: 2019-07-01 21:42:03
categories: Algorithm
tags: Other Algorithm
---

# 题目

在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

**示例 1:**
```java
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```

**示例 2::**
```java
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```

# 思路 + 代码

**简单办法，最容易想到的：用一个长度为 k 存储最大到第k大的数，然后返回数组最后一个元素，即为结果。**

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        // 数组初始化
        int[] results = new int[k];
        for(int i=0; i<k; i++){
            results[i] = Integer.MIN_VALUE;
        }
        // 特殊情况处理
        int len = nums.length;
        if(len<2) return nums[0];
        // 维护结果数组的值
        for(int i=0; i<len; i++){
            for(int j=0; j<k; j++){
                if(nums[i]>results[j]){
                    System.arraycopy(results,j,results,j+1,k-1-j);
                    results[j] = nums[i];
                    break;
                }
            }
        }
         return results[k-1];
    }
}
```

执行用时 :
74 ms, 在所有 Java 提交中击败了18.90%的用户

内存消耗 :
40.5 MB, 在所有 Java 提交中击败了35.28%的用户


**答案里的方法：桶排序，非常好理解，先遍历一遍数组找出最大最小值。创建一个桶，长度为max-min+1，桶的**

**引对应于与Min的差值，桶中装的元素为该值出现次数。**

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int max=Integer.MIN_VALUE;
        int min=Integer.MAX_VALUE;
        for(int num:nums){
            max=Math.max(max,num);
            min=Math.min(min,num);
        }
        int[] bucket=new int [max-min+1];
        for(int num:nums){
            bucket[num-min]++;
        }
        int count=0;
        for(int i=bucket.length-1;i>=0;i--){
            count+=bucket[i];
            if(count>=k)    return min+i;
        }
        return -1;
    }
}
```

执行用时 :
2 ms, 在所有 Java 提交中击败了99.61%的用户

内存消耗 :
37.8 MB, 在所有 Java 提交中击败了94.49%的用户