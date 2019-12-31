---
title: LeetCode 474.一和零
comments: true
toc: true
date: 2019-12-28 11:24:01
categories: Algorithm
tags: Dynamic Programming
---

# 题目

在计算机界中，我们总是追求用有限的资源获取最大的收益。

现在，假设你分别支配着 m 个 0 和 n 个 1。另外，还有一个仅包含 0 和 1 字符串的数组。

你的任务是使用给定的 m 个 0 和 n 个 1 ，找到能拼出存在于数组中的字符串的最大数量。每个 0 和 1 至多被使用一次。

注意:
```java
给定 0 和 1 的数量都不会超过 100。
给定字符串数组的长度不会超过 600。
```

示例 1:
```java
输入: Array = {"10", "0001", "111001", "1", "0"}, m = 5, n = 3
输出: 4

解释: 总共 4 个字符串可以通过 5 个 0 和 3 个 1 拼出，即 "10","0001","1","0" 。
```

示例 2:
```java
输入: Array = {"10", "0", "1"}, m = 1, n = 1
输出: 2
```
解释: 你可以拼出 "10"，但之后就没有剩余数字了。更好的选择是拼出 "0" 和 "1" 。

**题解**

多维背包，动态规划方程：`dp[i][j]=max(dp[i][j], dp[i-conut_0][j-count_1])`

```python
class Solution(object):
    def findMaxForm(self, strs, m, n):
        """
        :type strs: List[str]
        :type m: int
        :type n: int
        :rtype: int
        """
        if len(strs)==0:
            return 0
        dp = [[0]*(n+1) for _ in range(m+1)]
        for item in strs:
            count_0, count_1 = item.count('0'), item.count('1')
            for i in range(m, count_0-1, -1):
                for j in range(n, count_1-1, -1):
                    dp[i][j] = max(dp[i][j], dp[i-count_0][j-count_1]+1)
        return dp[-1][-1]
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ones-and-zeroes
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

# 零钱兑换

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

示例 1:
```java
输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1
```

示例 2:
```java
输入: coins = [2], amount = 3
输出: -1
```
说明:
你可以认为每种硬币的数量是无限的。

**题解**

背包问题，无限背包，从小到大遍历。

```python
class Solution(object):
    def coinChange(self, coins, amount):
        """
        :type coins: List[int]
        :type amount: int
        :rtype: int
        """
        dp = [float('inf')]*(amount+1) 
        dp[0] = [0]
        for coin in coins:
            for i in range(coin, amount+1):
                dp[i] = min(dp[i], dp[i-coin]+1)
        return dp[-1] if dp[-1] != float('inf') else -1
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/coin-change
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

# 零钱兑换 II

给定不同面额的硬币和一个总金额。写出函数来计算可以凑成总金额的硬币组合数。假设每一种面额的硬币有无限个。 

 

示例 1:
```java
输入: amount = 5, coins = [1, 2, 5]
输出: 4
解释: 有四种方式可以凑成总金额:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

示例 2:
```java
输入: amount = 3, coins = [2]
输出: 0
```
解释: 只用面额2的硬币不能凑成总金额3。

示例 3:
```java
输入: amount = 10, coins = [10] 
输出: 1
 ```

注意:

你可以假设：
0 <= amount (总金额) <= 5000
1 <= coin (硬币面额) <= 5000
硬币种类不超过 500 种
结果符合 32 位符号整数

**题解**

无限背包，注意递归条件是 `dp[i] += dp[i-coin]`

```python
class Solution(object):
    def change(self, amount, coins):
        """
        :type amount: int
        :type coins: List[int]
        :rtype: int
        """
        dp = [0]*(amount+1)
        dp[0] = 1
        for coin in coins:
            for i in range(coin, amount+1):
                dp[i] += dp[i-coin]
        return dp[-1]
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/coin-change-2
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

# 单词拆分

给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，判定 s 是否可以被空格拆分为一个或多个在字典中出现的单词。

说明：

拆分时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。
示例 1：
```java
输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。
```

示例 2：
```java
输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
```
解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。
     注意你可以重复使用字典中的单词。

示例 3：
```java
输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```

**题解**

采用动态规划 `dp[i]`表示 截止 s 字符串的 i 位置的子字符串是否能被单词字典里的单词来表示。

`dp[i]`为真的条件为：

1. `dp[j]`为真
2. `dp[j:i]`表示的字符串存在于字典中

```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: bool
        """
        # dp[i]表示s[i]是否可用wordDict里的元素表示
        # dp[i]能够被表示的前提是dp[j]可用wordDict里元素表示，而且dp[j:i]存在于wordDict中
        
        # 如果字典为空，字符串为空，则返回true，否则返回false
        if not wordDict: return not s
        size = len(s)
        dp = [False]*(size+1)
        wordDict = list(set(wordDict))
        dp[0] = True
        for i in range(1, size+1):
            for j in range(i-1, -1, -1):
                if dp[j] and dp[j:i] in wordDict:
                    dp[i] = True
                    break
        return dp[-1]
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/word-break
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

# 组合总和

给定一个由正整数组成且不存在重复数字的数组，找出和为给定目标正整数的组合的个数。

示例:

nums = [1, 2, 3]
target = 4

所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

请注意，顺序不同的序列被视作不同的组合。

因此输出为 7。
进阶：
如果给定的数组中含有负数会怎么样？
问题会产生什么变化？
我们需要在题目中添加什么限制来允许负数的出现？

**题解**

思路与零钱兑换一模一样，代码甚至都一样

```python
class Solution(object):
    def combinationSum4(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        if not nums: return 0
        size = len(nums)
        dp = [0]*(target+1)
        dp[0] = 1
        for i in range(1, target+1):
            for n in nums:
                if i>=n:
                    dp[i] += dp[i-n]
        return dp[-1]
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/combination-sum-iv
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。