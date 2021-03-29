---
title: LeetCode 160.相交链表
comments: true
toc: true
date: 2019-11-04 18:49:13
categories: 算法题
tags: LeetCode
---

# 题目

编写一个程序，找到两个单链表相交的起始节点。

# 思路 + 代码

**1. 最容易想到的，两层遍历求解。**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA==null || headB==null)
            return null;
        ListNode tmp;
        while(headA!=null){
            tmp = headB;
            while(tmp!=null){
                if(tmp!=headA)
                    return tmp;
                tmp = tmp.next;
            }
            headA = headA.next;
        }
        return null;
    }
}
```
时间复杂度O(M*N)
空间复杂度O(1)

**2. 利用Map记录一个链表的每个节点，第二个链表寻找第一次出现的节点。**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA==null || headB==null)
            return null;
        Map<ListNode,Integer> map = new HashMap<ListNode,Integer>();
        while(headA!=null){
            map.put(headA,1);
            headA = headA.next;
        }
        while(headB!=null){
            if(map.containsKey(headB))
                return headB;
            headB = headB.next;
        }
        return null;
    }
}
```

时间复杂度O(M+N)
空间复杂度O(M)或O(N)

**3. 或者两个指针，分别从两个链表的头结点开始，当一个节点遍历到尾部时换到另一个链表头部。**

即利用 a+all+b = b+all+a，也就是两个指针走的路程一样。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA==null || headB==null)
            return null;
        ListNode tmpA = headA;
        ListNode tmpB = headB;
        while(tmpA!=tmpB){
            tmpA = tmpA==null?headB:tmpA.next;
            tmpB = tmpB==null?headA:tmpB.next;
        }
        return tmpA;
        return tmpA;
    }
}
```

时间复杂度O(M+N)
空间复杂度O(1)