---
title: LeetCode 96.不同的二叉搜索树
comments: true
toc: true
date: 2019-11-12 16:44:44
categories: 算法题
tags: 动态规划
---

# 题目

给定一个整数 n，求以 1 ... n 为节点组成的二叉搜索树有多少种？

**示例:**
```java
输入: 3
输出: 5
解释:
给定 n = 3, 一共有 5 种不同结构的二叉搜索树:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

# 思路 + 代码

动态规划。

假设 整数 n对应的二叉搜索树数量为 G(n)。

每个节点 i ∈ (0,n] 为根节点对应的二叉搜索树数量为 F(i)。

则， G(n) = F(1) + F(2) + F(3) + ... + F(n)。

而 节点i 为根节点的二叉搜索树，可以分为 i-1 个左子树 跟 n-i个右子树，F(i) = G(i-1)*G(n-i);

因此 G(n) = G(0)*G(n-1) + G(1)*G(n-2) + G(2)*G(n-3) + ... + G(n-1)*G(0)

因此

```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n+1];
        dp[0]=1; // 边界条件
        dp[1]=1;
        for(int i=2; i<=n; i++){
            for(int j=1; j<=i; j++){
                // 节点i为根节点对应的二叉搜索树数量
                dp[i] += dp[j-1]*dp[i-j];
            }
        }
        return dp[n];
    }
}
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/unique-binary-search-trees
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
