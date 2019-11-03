---
title: LeetCode 543.二叉树的直径
comments: true
toc: true
date: 2019-11-03 18:22:01
categories: Algorithm
tags: Other Algorithm
---

# 题目

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过根结点。

**示例：**
给定二叉树
```java
          1
         / \
        2   3
       / \     
      4   5    
```

返回 3, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。

**注意：**两结点之间的路径长度是以它们之间边的数目表示。

# 思路 + 代码

路径长度一定是以某个节点为根节点，左右子树的最大深度和。

与题目[求二叉树的最大深度](http://sunyunzeng.com/%E7%AE%97%E6%B3%95%E9%A2%98-%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9B%B8%E5%85%B3/)类似

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    private int sum;
    public int diameterOfBinaryTree(TreeNode root) {
        if(root==null)
            return 0;
        getDepth(root);
        return sum;
    }
    private int getDepth(TreeNode root){
        if(root==null)
            return 0;
        int left = getDepth(root.left);
        int right = getDepth(root.right);
        sum = Math.max(sum,  left+right );
        return Math.max(left, right)+1;
    }
}
```

[题目链接](https://leetcode-cn.com/problems/diameter-of-binary-tree/)
