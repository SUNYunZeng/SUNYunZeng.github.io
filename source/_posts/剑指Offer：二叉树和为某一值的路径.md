---
title: 剑指Offer：二叉树和为某一值的路径
comments: true
toc: true
date: 2019-10-08 17:50:05
categories: Algorithm
tags: 
    - 剑指Offer
    - Binary Tree 
---

# 题目

输入一颗二叉树的根节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

# 思路 + 代码

递归求解，左右子树分别遍历（深度优先）。

遍历终止条件为节点为**null**或者是叶子节点且和为目标值。

否则回退列表，寻找更短的路径。

```java
import java.util.ArrayList;
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
    private ArrayList<ArrayList<Integer>> res;
    private ArrayList<Integer> list;
    public Solution(){
        res = new ArrayList<ArrayList<Integer>>();
        list = new ArrayList<Integer>();
    }
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int target) {
        if(root==null)
            return res;
        target = target - root.val;
        list.add(root.val);
        if(target==0&&root.right==null&&root.left==null){
            res.add(new ArrayList<Integer>(list));
        }
        FindPath(root.left, target);
        FindPath(root.right, target);
        list.remove(list.size()-1);
        return res;
    }
}
```

