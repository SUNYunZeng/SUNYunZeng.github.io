---
title: LeetCode 524.通过删除字母匹配到字典里最长单词
comments: true
toc: true
date: 2019-12-21 11:49:45
categories: Algorithm
tags: Other Algorithm
---

# 题目

给定一个字符串和一个字符串字典，找到字典里面最长的字符串，该字符串可以通过删除给定字符串的某些字符来得到。如果答案不止一个，返回长度最长且字典顺序最小的字符串。如果答案不存在，则返回空字符串。

**示例 1:**
```java
输入:
s = "abpcplea", d = ["ale","apple","monkey","plea"]

输出: 
"apple"
```
```java
示例 2:

输入:
s = "abpcplea", d = ["a","b","c"]

输出: 
"a"
```

说明:
所有输入的字符串只包含小写字母。
字典的大小不会超过 1000。
所有输入的字符串长度不会超过 1000。

# 思路 + 代码

快慢指针判断是否在字符串里面

```python
class Solution:
    def isInWord(self, a, b) -> bool:
        i, j = 0, len(b)-1
        m, n = 0, len(a)-1
        while i <= j:
            if m > n: return False
            if a[m]==b[i]:
                m+=1
                i+=1
            else:
                while m<=n and a[m]!=b[i]:
                    m+=1
                if m > n and i<=j: return False
            if a[n]==b[j]:
                n-=1
                j-=1
            else:
                while m<=n and a[n]!=b[j]:
                    n-=1
                if m > n and i<=j: return False
        return True
    def findLongestWord(self, s: str, d: List[str]) -> str:
        if len(s)==0 or len(d)==0: return ""
        res=""
        for i in range(len(d)):
            if self.isInWord(s, d[i]):
                if len(d[i])>len(res):
                    res = d[i]
                elif len(d[i])==len(res) and d[i]<res:
                    res = d[i]
        return res
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-word-in-dictionary-through-deleting
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
