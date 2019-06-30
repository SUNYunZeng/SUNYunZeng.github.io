---
title: Leetcode 234.回文链表
comments: true
toc: true
date: 2019-06-20 12:24:26
categories: Algorithm
tags: Other Algorithm
---

# 题目

请判断一个链表是否为回文链表。

**示例1：**
```java
输入: 1->2
输出: false
```

**示例2：**
```java
输入: 1->2->2->1
输出: true
```
**进阶：**
>你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

# 思路

用 **栈+快慢指针** 或者 **快慢指针+反转链表**

快慢指针是用来寻找中间节点。栈是用来反转链表。

# 代码

1. 栈 + 快慢指针

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isPalindrome(ListNode head) {
        if(head==null||head.next==null) return true;
        // 栈用来反转链表
        Stack<ListNode> stack = new Stack<>();
        ListNode fast = head;
        ListNode low = head;
        while(fast!=null&&fast.next!=null){
            fast = fast.next.next;      
            stack.push(low);
            low = low.next;
            // 奇数链表中间位置需要往后移动一位
            if(fast!=null&&fast.next==null)
                low = low.next;
        }
        // 循环遍历前后链表是否相等
        while(!stack.empty()){
            if(stack.pop().val!=low.val) 
                return false;
            low = low.next;
        }
        return true;
    }
}
```

2. 快慢指针 + 反转链表

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isPalindrome(ListNode head) {
        if(head==null||head.next==null) return true;
        ListNode fast = head;
        ListNode low = head;
        // 快慢指针找到中间点
        while(fast!=null&&fast.next!=null){
            fast = fast.next.next;      
            low = low.next;
            if(fast!=null&&fast.next==null)
                low = low.next;
        }
        ListNode compNode = reverseLinkedList(low);
        // 链表前后判断
        while(compNode!=null){
            if(compNode.val!=head.val)
                return false;
            compNode = compNode.next;
            head = head.next;
        }
        return true;
    }
    // 反转链表
    public ListNode reverseLinkedList(ListNode head){
        ListNode preNode = null;
        ListNode curNode = head;
        while(curNode!=null){
            ListNode temp = curNode.next;
            curNode.next = preNode;
            preNode = curNode;
            curNode = temp;
        }
        return preNode;
    }
}
```

[题目链接](https://leetcode-cn.com/problems/palindrome-linked-list/)
