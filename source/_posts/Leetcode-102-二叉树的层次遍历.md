---
title: Leetcode 102.二叉树的层次遍历
comments: true
toc: true
date: 2019-07-04 12:32:30
categories: Algorithm
tags: Binary Tree
---

# 题目

给定一个二叉树，返回其按层次遍历的节点值。 （即逐层地，从左到右访问所有节点）。

例如:
给定二叉树: [3,9,20,null,null,15,7],
```java
    3
   / \
  9  20
    /  \
   15   7
```

返回其层次遍历结果：

```java
[
  [3],
  [9,20],
  [15,7]
]
```

# 思路 + 代码

二叉树相关算法题两种解题思路：递归和迭代。


**递归方法**


用一个辅助函数，更新结果。

记录遍历的层数，并按照从左到右的顺序依次在相应层数List中记录数值。

返回结果。

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
    private List<List<Integer>> results = new ArrayList<List<Integer>>();
    public void helper(TreeNode root, int level){
        // 更新记录List
        if(results.size()==level)
            results.add(new ArrayList<Integer>());
        // 从左到右记录数据
        results.get(level).add(root.val);
        if(root.left!=null)
            helper(root.left, level+1);
        if(root.right!=null)
            helper(root.right, level+1);
    }
    public List<List<Integer>> levelOrder(TreeNode root) {
        if(root==null)
            return results;
        helper(root, 0);
        return results;
    }
}
```

**迭代方法:**

用队列辅助，先进的节点先出。

层层进队列，然后层层出队列，存入结果results中。

**代码:**
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
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> results = new ArrayList<List<Integer>>();
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        if(root==null)
            return results;
        queue.add(root);
        int level = 0;
        while(!queue.isEmpty()){
            results.add(new ArrayList<Integer>());
            int levelLength = queue.size();
            // 将level层元素依次存入List，level+1层节点依次入队列
            for(int i=0; i<levelLength; ++i){
                // 改成节点出队列
                root = queue.remove();
                // 按层存入
                results.get(level).add(root.val);
                // 下一层节点入队列
                if(root.left!=null)
                    queue.add(root.left);
                if(root.right!=null)
                    queue.add(root.right);
            }
            level++;
        }
        return results;
    }
}
```

[题目链接](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)