---
title: 剑指Offer：平衡二叉树
comments: true
toc: true
date: 2019-10-21 18:51:48
categories: Algorithm
tags: 剑指Offer
---

# 题目

输入一棵二叉树，判断该二叉树是否是平衡二叉树。

# 思路 + 代码

平衡二叉树的左右子树的高度差不大于 1。

```java
public class Solution {
    private boolean isBalanced = true;
    public boolean IsBalanced_Solution(TreeNode root) {
        height(root);
        return isBalanced;
    }
    private int height(TreeNode root){
        if(root==null || !isBalanced)
            return 0;
        int left = height(root.left);
        int right = height(root.right);
        if(1<Math.abs(left-right))
            isBalanced = false;
        return Math.max(left, right) + 1;
    }
}
```
