---
title: 剑指Offer：按之字形顺序打印二叉树
comments: true
toc: true
date: 2019-10-29 10:55:19
categories: 算法题
tags: 
    - 二叉树
    - 剑指Offer
---

# 题目

请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

# 思路 + 代码

其实就是[二叉树层次遍历](http://sunyunzeng.com/Leetcode-102-%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E6%AC%A1%E9%81%8D%E5%8E%86/)的变体。

遍历过程中，如果是二叉树的偶数层，就顺序遍历；否则逆序遍历。

```java
import java.util.ArrayList;

/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
import java.util.*;
public class Solution {
    public ArrayList<ArrayList<Integer> > Print(TreeNode pRoot) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        if(pRoot == null)
            return res;
        int layer = 0;
        int location = 0;
        queue.add(pRoot);
        while(!queue.isEmpty()){
            int len = queue.size();
            Integer[] nums = new Integer[len];
            for(int i=0;i<len; ++i){
                TreeNode tmp = queue.remove();
                if(layer%2!=0)
                    location = len-1-i;
                else
                    location = i;
                nums[location]=tmp.val;
                if(tmp.left!=null)
                    queue.add(tmp.left);
                if(tmp.right!=null)
                    queue.add(tmp.right);
            }
            ArrayList<Integer> list = new ArrayList<Integer>();
            Collections.addAll(list, nums);
            if(list.size()>0)
                res.add(list);
            layer++;
        }
        return res;
    }
}
```

