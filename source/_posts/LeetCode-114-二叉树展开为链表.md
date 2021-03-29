---
title: LeetCode 114.二叉树展开为链表
comments: true
toc: true
date: 2019-11-06 16:57:36
categories: 算法题
tags: 二叉树
---

# 题目

给定一个二叉树，原地将它展开为链表。

例如，给定二叉树：

```java
    1
   / \
  2   5
 / \   \
3   4   6
```

将其展开为：

```java
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```

# 题解 + 思路

一开始想的是递归，但是递归是由底向顶递归生成，而这道题是由顶到底生成，虽然存在子问题，但是仍难以求解。

其实可以看做如下步骤：

1. 找到左子树的最右节点。

```java
    1
   / \
  2   5
 / \   \
3   4   6
```

2. 将右子树移到左子树的最右节点。

```java
    1
   / 
  2   
 / \   
3   4
     \
      5
       \
        6
```

3. 右子树换为左子树，左子树置为 null

```java
    1
     \
      2   
     / \   
    3   4
         \
          5
           \
            6
```

4. 从右节点开始，继续该操作

```java
    1
     \
      2   
       \   
        4
         \
          5
           \
            6
             \
              3
```

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
    public void flatten(TreeNode root) {
       while(root!=null){
           if(root.left==null){
               root=root.right;
           }else{
               TreeNode pre  = root.left;
               while(pre.right!=null){
                   pre = pre.right;
               }
               pre.right = root.right;
               root.right = root.left;
               root.left = null;
               root = root.right;
           }
       }
    }
}
```

