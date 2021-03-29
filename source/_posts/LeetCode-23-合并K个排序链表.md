---
title: LeetCode 23.合并K个排序链表
comments: true
toc: true
date: 2019-11-03 19:11:27
categories: 算法题
tags: LeetCode
---

# 题目

合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

**示例：**
```java
输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6
```

# 思路 + 代码

[合并两个有序链表](http://sunyunzeng.com/%E7%AE%97%E6%B3%95%E9%A2%98-%E5%90%88%E5%B9%B6%E4%B8%A4%E4%B8%AA%E6%9C%89%E5%BA%8F%E9%93%BE%E8%A1%A8/)的翻版。

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
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists==null || lists.length==0)
            return null;
        if(lists.length==1) return lists[0];
        ListNode res = lists[0];
        for(int i=1; i<lists.length; i++){
            res = mergeTwoLists(res, lists[i]);
        }
        return res;
    }
    // 合并两个有序链表的操作
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1==null) return l2;
        if(l2==null) return l1;
        ListNode prehead = new ListNode(0);
        ListNode node = prehead;
        while(l1!=null && l2!=null){
            if(l1.val<=l2.val){
                node.next = l1;
                l1=l1.next;
            }else{
                node.next = l2;
                l2=l2.next;
            }
            node = node.next;
        }
        node.next=l1==null?l2:l1;
        return prehead.next; 
    }
}
```
