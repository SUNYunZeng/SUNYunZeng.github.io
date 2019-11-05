---
title: LeetCode 437.路径总和 III
comments: true
toc: true
date: 2019-11-05 17:16:41
categories: Algorithm
tags: Binary Tree
---

# 题目

给定一个二叉树，它的每个结点都存放着一个整数值。

找出路径和等于给定数值的路径总数。

路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。

**示例1：**
```java
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

返回 3。和等于 8 的路径有:

1.  5 -> 3
2.  5 -> 2 -> 1
3.  -3 -> 11
```

# 思路 + 代码

双重递归。

首先建立一个递归寻找以每个节点为根节点的路径查找。

再建立一个递归遍历每一个节点，并以该节点为根节点。

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
    private int cnt = 0;
    public int pathSum(TreeNode root, int sum) {
        helper(root, sum);
        return cnt;
    }
    private void helper(TreeNode root, int sum){
        if(root==null)
            return;
        search(root, sum, root.val);
        helper(root.left, sum);
        helper(root.right, sum);
    }
    private void search(TreeNode root, int sum, int tmp){
        if(sum==tmp){
            cnt++;
        }
        if(root.left!=null)
            search(root.left, sum, tmp+root.left.val);
        if(root.right!=null)
            search(root.right, sum, tmp+root.right.val);
    }
}
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/path-sum-iii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
