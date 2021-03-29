---
title: Leetcode 581.最短无序连续子数组
comments: true
toc: true
date: 2019-11-02 14:51:27
categories: 算法题
tags: LeetCode
---

# 题目

给定一个整数数组，你需要寻找一个连续的子数组，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

你找到的子数组应是最短的，请输出它的长度。

**示例1：**

```java
输入: [2, 6, 4, 8, 10, 9, 15]
输出: 5
解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
```

**说明：**

1. 输入的数组长度范围在 [1, 10,000]。

2. 输入的数组可能包含重复元素 ，所以升序的意思是<=。

# 思路 + 代码

首先用左右两个指针分别探测左右各自逆序的起点。此时序列对应的数值是局部最小值与局部最大值。

然后在**逆序序列**(左边逆序开始与右边逆序开始位置之间)中寻找全局最小值与全局最大值。

然后从左边开始遍历寻找小于等于全局最小值的边界；从右边寻找大于等于全局最大值的边界。

左右边界构成数组的长度即为所求。

```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        // 异常值处理
        if(nums==null||nums.length<=1)
            return 0;
        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;
        int len = nums.length;
        int i = 0, j = len-1;

        // 寻找左右逆序的边界
        while(i+1<len) if(nums[i+1]<nums[i++]) break;
        while(j-1>=0) if(nums[j-1]>nums[j--]) break;

        // 如果i+1>len，证明数组本身是升序的，返回0
        if(i+1>len) return 0;
        
        // 寻找全局最小值与全局最大值
        do{
            if(nums[i]<min) min = nums[i];
        }while(++i<len);
        do{
            if(nums[j]>max) max = nums[j];
        }while(--j>=0);

        // 从左右分别开始遍历，找到逆序数组真实左右边界
        i = 0;
        j = len-1;
        while(i<len && nums[] <=min) i++;
        while(j>=0 && nums[j] >=max) j--;
        
        return j-i+1>0?j-i+1:0;
    }
}
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。