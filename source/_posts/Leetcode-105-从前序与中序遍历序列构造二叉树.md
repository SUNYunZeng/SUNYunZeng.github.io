---
title: Leetcode 105.从前序与中序遍历序列构造二叉树
comments: true
toc: true
date: 2019-07-01 00:03:04
categories: Algorithm
tags: Other Algorithm
---

# 题目

根据一棵树的前序遍历与中序遍历构造出二叉树。

注意:
你可以假设树中没有重复的元素。

例如，给出

```java
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```java
    3
   / \
  9  20
    /  \
   15   7
```

# 思路

**先序遍历：先根节点 后左子树 最后右子树**

**中序遍历：先左子树 再根节点 最后右子树**

所以先序遍历的第一个数值为根节点，在中序遍历中找到根节点位置，前面为左子树的中序遍历，后面为右子树的中序遍历。

java代码太冗长了，不能很好地反映，所以采用更简洁的Python代码

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def buildTree(self, preorder, inorder):
        """
        :type preorder: List[int]
        :type inorder: List[int]
        :rtype: TreeNode
        """
        if not preorder:
            return None
        
        x = preorder.pop(0)
        node = TreeNode(x)
        idx = inorder.index(x)
        
        
        node.left = self.buildTree(preorder[:idx], inorder[:idx])
        node.right = self.buildTree(preorder[idx:], inorder[idx+1:])
        
        return node
```