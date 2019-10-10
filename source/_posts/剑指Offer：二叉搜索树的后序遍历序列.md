---
title: 剑指Offer：二叉搜索树的后序遍历序列
comments: true
toc: true
date: 2019-10-06 15:44:08
categories: Algorithm
tags: 剑指Offer
---

# 题目

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

# 思路 + 代码

**二叉搜索树**的左子树所有节点小于根节点，右子树的所有节点大于根节点。

**后序遍历**是先访问左子树，后访问右子树。

因此，根节点永远在序列的最后位置。找到根节点数值后，根据前面节点数值与根节点的大小关系，找到**左右子树的分割位置**，递归求解。

```java
public class Solution {
    public boolean VerifySquenceOfBST(int [] sequence) {
        // 初始条件判断
        if(sequence==null || sequence.length==0)
            return false;
        // 调用重写的辅助函数
        return VerifySquenceOfBST(sequence, 0, sequence.length-1);
    }
    private boolean VerifySquenceOfBST(int[] sequence, int start, int end){
        // 递归截止条件
        if(start >= end)
            return true;
        // 寻找分割节点
        int currentCutIndex = start;
        while(sequence[currentCutIndex]<sequence[end]){
            currentCutIndex++;
        }
        // 如果右子树存在小于根节点的节点，则该序列不是二叉搜索树的后序遍历
        for(int i=currentCutIndex; i<end; i++){
            if(sequence[i]<sequence[end])
                return false;
        }
        // 左右子树递归寻找
        return VerifySquenceOfBST(sequence, start, currentCutIndex-1) && 
            VerifySquenceOfBST(sequence, currentCutIndex, end-1);
    }
}
```
