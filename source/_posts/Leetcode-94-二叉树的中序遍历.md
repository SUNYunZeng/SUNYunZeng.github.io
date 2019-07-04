---
title: Leetcode 94.二叉树的中序遍历
comments: true
toc: true
date: 2019-07-03 17:11:32
categories: Algorithm
tags: Binary Tree
---

# 题目描述

给定一个二叉树，返回它的中序 遍历。

**示例:**
```java
输入: [1,null,2,3]
   1
    \
     2
    /
   3

输出: [1,3,2]
```

**进阶:**递归算法很简单，你可以通过迭代算法完成吗？

# 思路 + 代码

## 递归方法

递归截止条件：root==null

递归执行条件：        
    list.add(inorderTraversal(root.left))
    list.add(root.val)
    list.add(inorderTraversal(root.right))

**代码**

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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if(root==null) return list;
        List<Integer> 递归算法很简单，你可以通过迭代算法完成吗？
        List<Integer> rightList = inorderTraversal(root.right);
        for(int i=0; i<leftList.size(); i++){
            list.add(leftList.get(i));
        }
        list.add(root.val);
        for(int i=0; i<rightList.size(); i++){
            list.add(rightList.get(i));
        }
        return list;
    }
}
```

## 迭代方法

用一个栈存储遍历过得父子节点，循环遍历。

**代码**

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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        // 当树没有遍历完全
        while(root!=null || !stack.isEmpty()){
            // 找到当前树的最左侧叶子节点（父节点）
            while(root!=null){
                stack.push(root);
                root=root.left;
            }
            // 中序遍历
            root = stack.pop();
            list.add(root.val);
            root = root.right;
        }
        return list;
    }
}
```

[题目链接](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/comments/)
