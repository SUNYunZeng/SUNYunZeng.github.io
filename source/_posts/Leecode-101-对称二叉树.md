---
title: Leetcode 101.对称二叉树
comments: true
toc: true
date: 2019-06-25 21:52:19
categories: 算法题
tags: 二叉树
---

# 题目

给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
```java
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

```java
   1
   / \
  2   2
   \   \
   3    3
```

**说明:**

如果你可以运用递归和迭代两种方法解决这个问题，会很加分。

# 思路

二叉树的一个典型套路就是递归求解。左右树分别对待。

注意递归截止条件以及是否对称的判断条件。

# 代码

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
    public boolean isSymmetric(TreeNode root) {
        if(root==null)
            return true;
        return tryTree(root.left, root.right);
    }
    public boolean tryTree(TreeNode left, TreeNode right){
        // 递归截止条件
        if(right==null&&left==null)
            return true;
        if(left==null||right==null)
            return false;
        // 对称判断条件
        if(left.val==right.val)
            return tryTree(left.left, right.right)&&tryTree(left.right, right.left);
        return false;
    }
}
```

