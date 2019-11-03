---
title: 剑指Offer：二叉搜索树与双向链表
comments: true
toc: true
date: 2019-10-10 14:04:38
categories: Algorithm 
tags: 
    - 剑指Offer
    - Binary Search Tree
---

# 题目

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

# 思路 + 代码

二叉搜索树的<u>[中序遍历](http://sunyunzeng.com/Leetcode-94-%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E4%B8%AD%E5%BA%8F%E9%81%8D%E5%8E%86/)</u>即为一个排序的顺序，按照该顺序依次构建双向链表即可。

```java
/**
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
    private TreeNode firstHead = null;
    private TreeNode preHead = null;
    public TreeNode Convert(TreeNode pRootOfTree) {
        if(pRootOfTree==null)
            return null;
        // 整个遍历过程与中序遍历相同
        // 遍历到最左侧叶子节点，即最小的叶子节点
        Convert(pRootOfTree.left);
        // 第一次将头指针指向最小叶子节点（处理中间节点pRootOfTree）
        if(preHead==null){
            preHead = pRootOfTree;
            firstHead = pRootOfTree;
        }else{
            pRootOfTree.left=preHead;
            preHead.right = pRootOfTree;
            preHead = pRootOfTree;
        }
        // 右节点
        Convert(pRootOfTree.right);
        return firstHead;
    }
}
```
