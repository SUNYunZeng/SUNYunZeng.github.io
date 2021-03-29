---
title: 剑指Offer：删除链表中重复的节点
comments: true
toc: true
date: 2019-10-27 22:42:07
categories: 算法题
tags: 剑指Offer
---

# 题目

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

# 思路 + 代码

利用递归求解，问题分解为去除下一个节点开始链表中重复的节点。

```java
/*
 public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}
*/
import java.util.*;
public class Solution {
    public ListNode deleteDuplication(ListNode pHead)
    {
        if(pHead==null || pHead.next==null)
            return pHead;
        ListNode next = pHead.next;
        if(pHead.val==next.val){
            while(next!=null && pHead.val == next.val)
                next = next.next;
            return deleteDuplication(next);
        }else{
            pHead.next = deleteDuplication(pHead.next);
            return pHead;
        }
    }
}
```
