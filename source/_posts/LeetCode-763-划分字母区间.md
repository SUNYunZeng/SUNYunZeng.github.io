---
title: LeetCode 763. 划分字母区间
comments: true
toc: true
date: 2019-12-20 19:56:57
categories: Algorithm
tags: Greedy Algorithm
---

# 题目

字符串 S 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一个字母只会出现在其中的一个片段。返回一个表示每个字符串片段的长度的列表。

**示例1：**
```java
输入: S = "ababcbacadefegdehijhklij"
输出: [9,7,8]
解释:
划分结果为 "ababcbaca", "defegde", "hijhklij"。
每个字母最多出现在一个片段中。
像 "ababcbacadefegde", "hijhklij" 的划分是错误的，因为划分的片段数较少。
```

**注意：**
```java
S的长度在[1, 500]之间。
S只包含小写字母'a'到'z'。
```

# 思路 + 代码

贪心算法。

统计每个字母出现的最后位置，最优分割是该区间内任意一个字母的最大位置都包括在内。

```python
class Solution:
    def partitionLabels(self, S: str) -> List[int]:
        dic = {c:i for i, c in enumerate(S)}
        j, anchor = 0, 0
        res = []
        for i, c in enumerate(S):
            j = max(j, dic[c])
            if i==j:
                res.append(i-anchor+1)
                anchor = i+1
        return res
```
