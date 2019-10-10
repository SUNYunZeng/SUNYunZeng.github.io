---
title: 剑指Offer：复杂链表的复制
comments: true
toc: true
date: 2019-10-10 10:27:46
categories: Algorithm
tags: 剑指Offer
---

# 题目

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

# 思路 + 代码

参考 <u>https://github.com/CyC2018/CS-Notes/blob/master/notes/%E5%89%91%E6%8C%87%20Offer%20%E9%A2%98%E8%A7%A3%20-%2030~39.md#35-%E5%A4%8D%E6%9D%82%E9%93%BE%E8%A1%A8%E7%9A%84%E5%A4%8D%E5%88%B6</u>

分为三步：

**1. 在每个节点的后面插入复制的节点。**

{% asset_img step1.png %}

**2. 每个复制的节点 random 赋值。**

{% asset_img step2.png %}

**3. 拆分。**

{% asset_img step3.png %}

```java
/*
public class RandomListNode {
    int label;
    RandomListNode next = null;
    RandomListNode random = null;

    RandomListNode(int label) {
        this.label = label;
    }
}
*/
public class Solution {
    public RandomListNode Clone(RandomListNode pHead)
    {
        if(pHead == null)
            return null;
        RandomListNode cur = pHead;
        // 先在链表中间插入节点
        while(cur!=null){
            RandomListNode tmp = new RandomListNode(cur.label);
            tmp.next = cur.next;
            cur.next = tmp;
            cur = tmp.next;
        }
        // 然后中间节点的 random 指针赋值
        cur = pHead;
        while(cur!=null){
            if(cur.random!=null)
                // 这里是当前节点 random 链接的下一个为复制节点的 random 链接
                cur.next.random = cur.random.next;
            cur = cur.next.next;
        }
        // 两个链表分离
        cur = pHead;
        RandomListNode res = cur.next;
        while(cur.next!=null){
            RandomListNode tmp = cur.next;
            cur.next = tmp.next;
            cur = tmp;
        }
        return res;
    }
}
```