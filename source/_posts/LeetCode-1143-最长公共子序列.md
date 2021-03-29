---
title: LeetCode 1143.最长公共子序列
comments: true
toc: true
date: 2019-12-27 17:04:47
categories: 算法题
tags: 动态规划
---

# 最长公共子序列

给定两个字符串 text1 和 text2，返回这两个字符串的最长公共子序列。

一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。

若这两个字符串没有公共子序列，则返回 0。


示例 1:
```java
输入：text1 = "abcde", text2 = "ace" 
输出：3  
```
解释：最长公共子序列是 "ace"，它的长度为 3。
示例 2:
```java
输入：text1 = "abc", text2 = "abc"
输出：3
```
解释：最长公共子序列是 "abc"，它的长度为 3。
示例 3:
```java
输入：text1 = "abc", text2 = "def"
输出：0
```
解释：两个字符串没有公共子序列，返回 0。
 

提示:
```java
1 <= text1.length <= 1000
1 <= text2.length <= 1000
```
输入的字符串只含有小写英文字符。

**题解**

动态规划，不过要设计一个二维数组dp，来统计`dp[i][j]`时对应 text1 在 i 位置与text2 在 j 位置时两子字符串对应的最长公共子序列。

```python
class Solution(object):
    def longestCommonSubsequence(self, text1, text2):
        """
        :type text1: str
        :type text2: str
        :rtype: int
        """
        l1, l2 = len(text1), len(text2)
        if l1==0 or l2==0: return 0
        dp = [[0 for i in range(l2+1)] for i in range(l1+1)]
        for i in range(1,l1+1):
            for j in range(1,l2+1):
                if text1[i-1]==text2[j-1]:
                    dp[i][j] = dp[i-1][j-1]+1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        return dp[l1][l2]
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-common-subsequence
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

# 两个字符串的删除操作(最长公共子序列的变体)

给定两个单词 word1 和 word2，找到使得 word1 和 word2 相同所需的最小步数，每步可以删除任意一个字符串中的一个字符。

示例 1:

输入: "sea", "eat"
输出: 2
解释: 第一步将"sea"变为"ea"，第二步将"eat"变为"ea"
说明:

给定单词的长度不超过500。
给定单词中的字符只含有小写字母。

## 思路 + 代码

最长公共子序列的变体，其实就是求最长公共自序列。

```python
class Solution(object):
    def minDistance(self, word1, word2):
        """
        :type word1: str
        :type word2: str
        :rtype: int
        """
        m, n = len(word1), len(word2)
        dp = [[0]*(n+1) for _ in range(m+1)]
        for i in range(1, m+1):
            for j in range(1,n+1):
                if word1[i-1]==word2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        return m+n-2*dp[m][n]
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/delete-operation-for-two-strings
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

# 分割等和子集

给定一个只包含正整数的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

注意:

每个数组中的元素不会超过 100
数组的大小不会超过 200

示例 1:
```java
输入: [1, 5, 11, 5]
输出: true

解释: 数组可以分割成 [1, 5, 5] 和 [11].
 ```

示例 2:
```java
输入: [1, 2, 3, 5]
输出: false

解释: 数组不能分割成两个元素和相等的子集.
 ```

**题解**

这是一道以 0-1 背包问题为背景的算法练习题，我们把这个题目翻译一下：

给定一个只包含正整数的非空数组。是否可以从这个数组中挑选出一些正整数，每个数只能用一次，使得这些数的和等于整个数组元素的和的一半。

0-1 背包问题也是最基础的背包问题，它的特点是：待挑选的物品有且仅有一个，可以选择也可以不选择。下面我们定义状态，不妨就用问题的问法定义状态试试看。

> dp[i][j]：表示从数组的 [0, i] 这个子区间内挑选一些正整数，每个数只能用一次，使得这些数的和等于 j。

根据我们学习的 0-1 背包问题的状态转移推导过程，新来一个数，例如是 nums[i]，根据这个数可能选择也可能不被选择：

如果不选择 nums[i]，在 [0, i - 1] 这个子区间内已经有一部分元素，使得它们的和为 j ，那么 dp[i][j] = true；
如果选择 nums[i]，在 [0, i - 1] 这个子区间内就得找到一部分元素，使得它们的和为 j - nums[i] ，我既然这样写出来了，你就应该知道，这里讨论的前提条件是 nums[i] <= j。

以上二者成立一条都行。于是得到状态转移方程是：

```java
dp[i][j] = dp[i - 1][j] or dp[i - 1][j - nums[i]], (nums[i] <= j)
```
于是按照 0-1 背包问题的模板，我们不难写出以下代码。

```python
class Solution(object):
    def canPartition(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        size = len(nums)
        s = sum(nums)
        if s & 1 == 1:
            return False
        target = s // 2
        dp = [[False for _ in range(target+1)] for _ in range(size)]
        for i in range(target+1):
            if nums[0]==i:
                dp[0][i] = True
        for i in range(1,size):
            n = nums[i]
            for j in range(target+1):
                if j>=n:
                    dp[i][j] = dp[i-1][j] or dp[i][j-n]
                else:
                    dp[i][j] = dp[i-1][j]
        return dp[-1][-1]
```

优化版：
```python
class Solution(object):
    def canPartition(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        size = len(nums)
        s = sum(nums)
        if s & 1 == 1:
            return False
        target = s // 2
        dp = [False for _ in range(target+1)]
        for i in range(target+1):
            if nums[0]==i:
                dp[i] = True
        for i in range(1,size):
            n = nums[i]
            for j in range(target, n-1, -1):
                if j>=n:
                    dp[j] = dp[j] or dp[j-n]
        return dp[-1]
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/partition-equal-subset-sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。