---
title: 剑指Offer 树的子结构
comments: true
toc: true
date: 2019-09-29 16:33:23
categories: Algorithm
tags: 剑指Offer
---

# 树的子结构

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

# 思路 + 代码

涉及到二叉树相关的问题，首先想到是剑指O剑指递归。

由于二叉树是否为子树的判断，涉及到是否为 <font color=#f07c82>null</font>的判断，因此需要建立子判定函数，避免空树情况产生干扰。

判断条件：

**如果当前树节点与待判断树节点数值相同，则继续进行左右树的数值判断。直到两树中某一树为空。**

**否则分别对当前树的子树与右树跟带判断子树进行判定。**

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
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        if(root1==null || root2==null)
            return false;
        // 不能只判断当前第一个节点，如果首节点是伪相等，就是错误的
        return isSubtree(root1, root2)||
            isSubtree(root1.left, root2)||isSubtree(root1.right, root2);
    }
    private boolean isSubtree(TreeNode root1,TreeNode root2){
        if(root2==null)
            return true;
        if(root1==null)
            return false;
        if(root1.val==root2.val){
           return (isSubtree(root1.left, root2.left) && isSubtree(root1.right, root2.right)); 
        }else{
            return (isSubtree(root1.left, root2) || isSubtree(root1.right, root2));
        }
    }
}
```