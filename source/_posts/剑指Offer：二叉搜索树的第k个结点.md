---
title: 剑指Offer：二叉搜索树的第k个结点
comments: true
toc: true
date: 2019-10-30 11:06:47
categories: 算法题
tags: 
    - 剑指Offer
    - 二叉树
---

# 题目

给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）    中，按结点数值大小顺序第三小结点的值为4。

# 思路 + 代码

其实就是[二叉搜索树的中序遍历](http://sunyunzeng.cn/Leetcode-94-%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E4%B8%AD%E5%BA%8F%E9%81%8D%E5%8E%86/)翻版。

```java
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    private int cnt;
    private TreeNode node;
    TreeNode KthNode(TreeNode pRoot, int k)
    {
        if(k<1)
            return null;
        cnt = 0;
        inOrder(pRoot, k);
        return node;
    }
    private void inOrder(TreeNode pRoot, int k){
        if(cnt>=k || pRoot==null)
            return;
        inOrder(pRoot.left, k);
        cnt++;
        if(cnt==k){
            node = pRoot;
        }
        inOrder(pRoot.right, k);
    }
}
```
