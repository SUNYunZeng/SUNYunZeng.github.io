---
title: LeetCode 560.和为K的子数组
comments: true
toc: true
date: 2019-11-03 11:17:21
categories: 算法题
tags: LeetCode
---

# 题目

给定一个整数数组和一个整数 k，你需要找到该数组中和为 k 的连续的子数组的个数。

**示例1：**
```java
输入:nums = [1,1,1], k = 2
输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。
```

**说明：**
1. 数组的长度为 [1, 20,000]。

2. 数组中元素的范围是 [-1000, 1000] ，且整数 k 的范围是 [-1e7, 1e7]。

## 思路1 

先固定数组的左侧位置，然后依次移动右侧指针，如何数组和为 k，结果加一。

数组和置为零，更新左侧位置。

```java
class Solution {
    public int subarraySum(int[] nums, int k) {  
       int res=0;
       for(int left=0; left<nums.length; left++){
           int sum=0;
           for(int right = left; right<nums.length; right++){
               sum += nums[right];
               if(sum==k)
                    res++;
           }
       }
       return res;
    }
}
```

此时空间复杂度为 1， 时间复杂度为 O(n)。

## 思路2

只需要遍历一次，统计到各个位置的累积和。

如果至 j 位置的和与至 i 位置的和差为k，那么位于 i~j 的子序列和为 k。

用一个Map统计不同累积和出现的次数。

```java
class Solution {
    public int subarraySum(int[] nums, int k) {  
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        int sum = 0, res = 0;
        for(int n: nums){
            sum += n;
            if(map.containsKey(sum-k))
                res += map.get(sum-k);
            // map.getOrDefault(key, default)--如果存在键K就取出对应的V，否则值为default
            map.put(sum, map.getOrDefault(sum, 0)+1);
        }
    }
}
```

