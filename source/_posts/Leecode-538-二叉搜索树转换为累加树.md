---
title: Leetcode 538.二叉搜索树转换为累加树
comments: true
toc: true
date: 2019-06-17 11:55:59
categories: Algorithm
tags: Binary Tree
---

# 题目

给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。

二叉搜索树：它或者是一棵空树，或者是具有下列性质的二叉树： 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值； 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值； 它的左、右子树也分别为二叉搜索树。

# 思路

因为二叉搜索树左子树、根节点及右子树已经拍好顺序，所以只需遍历右子树计算累加值，然后对根节点与左子树分别累加。

可采用递归或遍历方法。首先累加右子树数值，然后依次修改根节点与左子树的数值。

# 代码1 递归

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
    private int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if(root == null) return null;
        convertBST(root.right);
        root.val +=sum;
        sum = root.val;
        convertBST(root.left);
        return root;
    }
}
```

# 代码2 遍历

```JAVA
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
    private int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        Stack<TreeNode> rightNodeStack = new Stack<TreeNode>();
        TreeNode node = root;
        while(node!=null || !rightNodeStack.isEmpty()){
            while(node!=null){
                rightNodeStack.add(node);
                node = node.right;
            }
            node = rightNodeStack.pop();
            node.val += sum;
            sum = node.val;
            node = node.left;
        }
        return root;
    }
}
```

[题目链接](https://leetcode-cn.com/problems/convert-bst-to-greater-tree)

