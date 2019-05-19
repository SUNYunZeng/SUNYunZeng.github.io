---
title: Algorithm--Fast and Slow Pointer
toc: true
date: 2019-05-17 22:00:47
categories: Algorithm
tags: Fast and Slow Pointer
---
# **快慢指针的应用**

## **判断链表是否存在环**

**Title Detail**

Given a linked list, determine if it has a cycle in it.

To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.

>Example 1:

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

{% asset_img  circularlinkedlist.png %}

### **思路**

利用快慢指针，快指针每次走两步，慢指针每次走一步。如果快指针能够追上慢指针，则链表存在环。

### **Algorithm**
```Java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head==null || head.next==null || head.next.next==null) return false;
        ListNode fast = head;
        ListNode slow = head;
        while(fast.next!=null && fast.next.next!=null){
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow){
                return true;
            }
        }
        return false;
    }
}
```
[题目链接](https://leetcode-cn.com/problems/linked-list-cycle/)

## **有序数组移除重复数字**

**Title Detail**

Given a sorted array nums, remove the duplicates **in-place** such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

**Example 1:**
```
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.
```

**Example 1:**
```
Given nums = [0,0,1,1,1,2,2,3,3,4],

Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

It doesn't matter what values are set beyond the returned length.
```

**Clarification:**
Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by reference, which means modification to the input array will be known to the caller as well.

Internally you can think of this:
```java
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

### **思路**

前提是有序数组，可见排序是很基础且重要的算法！

>过程：
>1. 初始，快慢指针指向第一个数字。
>2. 当快慢指针指向的数字都相同，快指针往前移动一位。
>3. 当快慢指针指向的数字不相同，慢指针往前移动一位，把快指针指向的数字赋予慢指针，快指针往前移动一位。
>4. 慢指针移动的 **位数+1** 即为不同数字的数量。且数组从初始位置到慢指针指向的位置，数字都是排好序的。

{% asset_img  remove-duplicates-from-sorted-array.gif %}

### **Algorithm**
```Java
class Solution {
    public int removeDuplicates(int[] nums) {
        int fast=0;
        int slow=0;
        while(fast<nums.length){
            if(nums[fast]==nums[slow]){
                fast++;
            }else{
                nums[++slow]=nums[fast++];
            }
        }
        return slow+1;
    }
}
```

1. [图片地址]( https://github.com/MisterBooo/LeetCodeAnimation)   

2. [题目链接](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)

