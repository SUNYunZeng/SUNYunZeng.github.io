---
title: LeetCode 300.最长上升子序列
comments: true
toc: true
date: 2019-12-26 10:04:32
categories: Algorithm
tags: Dynamic Programming
---

# 最长上升子序列

给定一个无序的整数数组，找到其中最长上升子序列的长度。

示例:
```java
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
```

**说明:**

可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
你算法的时间复杂度应该为 O(n2) 。

**进阶:** 你能将算法的时间复杂度降低到 O(n log n) 吗?

```python
class Solution(object):
    def lengthOfLIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        l = len(nums)
        if l==0 or l==1: return l
        dp = [1]*l
        for i in range(1, l):
            for j in range(0, i):
                if nums[i]>nums[j]:
                    dp[i] = max(dp[i], dp[j]+1)
        return max(dp)
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-increasing-subsequence
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

# 最长数对链

给出 n 个数对。 在每一个数对中，第一个数字总是比第二个数字小。

现在，我们定义一种跟随关系，当且仅当 b < c 时，数对(c, d) 才可以跟在 (a, b) 后面。我们用这种形式来构造一个数对链。

给定一个对数集合，找出能够形成的最长数对链的长度。你不需要用到所有的数对，你可以以任何顺序选择其中的一些数对来构造。

示例 :
```java
输入: [[1,2], [2,3], [3,4]]
输出: 2
解释: 最长的数对链是 [1,2] -> [3,4]
```

**注意：**

给出数对的个数在 [1, 1000] 范围内。

更上面一题类似，只不要是任意顺序的选择，可以先对列表进行排序，按照数对的第二个数值。

```python
class Solution(object):
    def findLongestChain(self, pairs):
        """
        :type pairs: List[List[int]]
        :rtype: int
        """
        l = len(pairs)
        if l==0 or l==1: return l
        pairs = sorted(pairs, key=lambda x: x[1])
        dp = [1]*l
        for i in range(1,l):
            for j in range(0, i):
                if pairs[i][0]>pairs[j][1]:
                    dp[i] = max(dp[i], dp[j]+1);
        return max(dp)
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximum-length-of-pair-chain
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

# 摆动序列

如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为摆动序列。第一个差（如果存在的话）可能是正数或负数。少于两个元素的序列也是摆动序列。

例如， [1,7,4,9,2,5] 是一个摆动序列，因为差值 (6,-3,5,-7,3) 是正负交替出现的。相反, [1,4,7,2,5] 和 [1,7,4,5,5] 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。

给定一个整数序列，返回作为摆动序列的最长子序列的长度。 通过从原始序列中删除一些（也可以不删除）元素来获得子序列，剩下的元素保持其原始顺序。

示例 1:
```java
输入: [1,7,4,9,2,5]
输出: 6 
解释: 整个序列均为摆动序列。
```

示例 2:
```java
输入: [1,17,5,10,13,15,10,5,16,8]
输出: 7
解释: 这个序列包含几个长度为 7 摆动序列，其中一个可为[1,17,10,13,10,16,8]。
```

示例 3:
```java
输入: [1,2,3,4,5,6,7,8,9]
输出: 2
```

**算法**

首先计算得到差值序列，例如：
```java
[2,5,3, 1] -> [1, -2, -2]
```
然后去重：
```java
[2,5,3, 1] -> [1, -2]
// 注意：全零序列的结果为1
[0,0,0] -> [0]
```
动态规划计算，dp[i]保存的是截止位置i的最长摆动序列。
```python
class Solution(object):
    # 判断i位置的元素可以添加到j元素的尾部，构成摆动序列
    def isSuccess(self, nums, i, j):
        if nums[i] > 0 > nums[j] or nums[i] < 0 < nums[j]:
            return True
        else:
            return False

    def wiggleMaxLength(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        l = len(nums)
        if l == 0 or l == 1: return l
        if l == 2: return 1
        dp, _nums = [1] * (l - 1), [1] * (l - 1)
        for i in range(1, l):
            _nums[i - 1] = nums[i] - nums[i - 1]
        # 差序列去重
        nums = list(set(_nums))
        nums.sort(key=_nums.index)
        # 防止全零序列的干扰
        if len(nums) == 1 and nums[0] == 0: return 1
        # 动态规划求解
        dp = [1] * len(_nums)
        for i in range(1, len(_nums)):
            if _nums[i] == 0: continue
            for j in range(0, i):
                if self.isSuccess(_nums, i, j):
                    dp[i] = max(dp[i], dp[j] + 1)
        return max(dp) + 1
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/wiggle-subsequence
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。