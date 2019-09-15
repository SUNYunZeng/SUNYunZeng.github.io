---
title: Leetcode 33.搜索旋转排序数组
comments: true
toc: true
date: 2019-09-15 20:50:03
categories: Algorithm
tags: Other Algorithm
---

# 题目

假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 O(log n) 级别。

**示例1:**
```java
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
```

**示例2:**
```java
输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1
```

# 思路+代码

时间复杂度要求是O(log n)，也就是二分法。于是我写了一个最基础的二分法...还是用递归..

```java
class Solution {
    public int search(int[] nums, int target) {
        if(nums.length==0 || nums==null)
            return -1;
        if(nums.length==1)
            return nums[0]==target?0:-1;
        return subSearch(nums, target, 0, nums.length-1);
    }
    private int subSearch(int[] nums, int target, int left, int right){
        if(nums[left]==target)
            return left;
        if(nums[right]==target)
            return right;
        int mid = (left+right)/2;
        if(nums[mid]==target)
            return mid;
        if(right-left==1)
            return -1;
        int tmp=-1;
        
        tmp=subSearch(nums, target, left, mid);
            
        if(tmp!=-1)
            return tmp;
        tmp=subSearch(nums, target, mid, right);
        
        return tmp;
    }
}
```

看了答案之后... 我想说:****

```java
class Solution {
    public int search(int[] nums, int target) {
       int left = 0, right = nums.length-1;
        while(left<=right){
            int mid=(left+right)/2;
            if(target==nums[mid])
                return mid;
            // 左侧不包含旋转序列
            if(nums[left]<=nums[mid]){
                if(target>=nums[left]&&target<nums[mid]){
                    right = mid-1;
                }else{
                    left = mid+1;
                }                
            }
            // 右侧不包含旋转序列
            else{
                if(target>nums[mid]&&target<=nums[right]){
                    left = mid+1;
                }else{
                    right = mid-1;
                }
            }        
        }
        return -1;
    }
}
```
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/search-in-rotated-sorted-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。