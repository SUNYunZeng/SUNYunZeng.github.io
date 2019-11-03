---
title: 剑指Offer：二叉树的下一个结点
comments: true
toc: true
date: 2019-10-28 20:44:51
categories: Algorithm
tags: 
    - Binary Tree
    - 剑指Offer
---

# 题目

给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

# 思路 + 代码

树的问题最容易想到的就是递归方法。

中序遍历顺序是左叶子节点、根节点、右叶子节点。

分为两种情况：

1. 如果右子节点不为空，则递归寻找后面最左叶子节点。

2. 如果右子节点为空，则递归在父级节点寻找。

    2.1 如果父节点为空，则返回 null。

    2.2 如果父节点左节点等于该节点，则返回父节点。

    2.3 如果父节点右节点等于该节点，则继续递归寻找。

```java
/*
public class TreeLinkNode {
    int val;
    TreeLinkNode left = null;
    TreeLinkNode right = null;
    TreeLinkNode next = null;

    TreeLinkNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public TreeLinkNode GetNext(TreeLinkNode pNode)
    {
        if(pNode == null)
            return null;
        if(pNode.right!=null)
            return getRightNode(pNode.right);
        else
            return getParentNode(pNode);
    }
    // 寻找最左节点
    private TreeLinkNode getRightNode(TreeLinkNode pNode){
        if(pNode.left==null)
            return pNode;
        else
            return getRightNode(pNode.left);
    }
    // 如果没有右节点，下一节点则是父级节点
    private TreeLinkNode getParentNode(TreeLinkNode pNode){
        if(pNode.next==null)
            return null;
        if(pNode.next.left == pNode)
            return pNode.next;
        else
            return getParentNode(pNode.next);
    }
}
```
