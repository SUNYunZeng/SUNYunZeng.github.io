---
title: 剑指Offer：从上往下打印二叉树
comments: true
toc: true
date: 2019-10-06 10:00:41
categories: Algorithm
tags: 剑指Offer
---

# 题目

从上往下打印出二叉树的每个节点，同层节点从左至右打印。

# 思路 + 代码

二叉树的层次遍历，用队列保存同一层次的树节点，然后依次遍历。

类似题目<u>http://sunyunzeng.com/Leetcode-102-%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E6%AC%A1%E9%81%8D%E5%8E%86/</u>

```java
import java.util.*;
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
    public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
        ArrayList<Integer> list = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        if(root==null)
          return list;
        queue.add(root);
        while(!queue.isEmpty()){
            int len = queue.size();
            for(int i=0; i<len; i++){
                root = queue.remove();
                list.add(root.val);
                if(root.left!=null)
                    queue.add(root.left);
                if(root.right!=null)
                    queue.add(root.right);
            }
        }
        return list;
    }
}
```


