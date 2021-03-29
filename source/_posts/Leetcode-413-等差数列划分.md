---
title: Leetcode 413.等差数列划分
comments: true
toc: true
date: 2019-12-25 11:21:09
categories: 算法题
tags: 动态规划
---

# 等差数列划分

如果一个数列至少有三个元素，并且任意两个相邻元素之差相同，则称该数列为等差数列。

例如，以下数列为等差数列:

1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9

以下数列不是等差数列。

1, 1, 2, 5, 7
 
```java
数组 A 包含 N 个数，且索引从0开始。数组 A 的一个子数组划分为数组 (P, Q)，P 与 Q 是整数且满足 0<=P<Q<N 。
```
如果满足以下条件，则称子数组(P, Q)为等差数组：
```java
元素 A[P], A[p + 1], ..., A[Q - 1], A[Q] 是等差的。并且 P + 1 < Q 。
```
函数要返回数组 A 中所有为等差数组的子数组个数。

 

示例:

A = [1, 2, 3, 4]

返回: 3, A 中有三个子等差数组: [1, 2, 3], [2, 3, 4] 以及自身 [1, 2, 3, 4]。

# 思路 + 代码

首先对于等差序列 B, 其元素数量为n，则其包含的连续自等差序列的总数为 1+2+...+n-2, 
因此，该题转化为寻找序列中，**最长的连续子等差序列**，然后根据其数量判断。

```python
class Solution:
    def numberOfArithmeticSlices(self, A: List[int]) -> int:
        l = len(A)
        if l < 3: return 0
        res, count = 0, 0
        for i in range(2,l):
            if A[i]-A[i-1] == A[i-1]-A[i-2]:
                count+=1
            else:
                if count!=0:
                    res += sum(range(count+1))
                    count=0
        if count != 0:
            res += sum(range(count + 1))
        return res
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/arithmetic-slices
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

# 题目

给定一个正整数 n，将其拆分为至少两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。

示例 1:
```java
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。
```
示例 2:
```java
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
```

说明: 你可以假设 n 不小于 2 且不大于 58。

# 思路 + 代码

动态规划，整数4的最大乘积为: dp[3] = max(max(dp[2], dp[1]*2), 1 * 2)

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        if n < 2: return 0
        dp = [1] * (n+1)
        for i in range(2, n + 1):
            for j in range(1, i):
                dp[i] = max(dp[j] * (i - j), dp[i])
                # 很关键的一步，因为之前的dp[i]最大值可能比dp[i]小，例如2或者3
                dp[i] = max(j*(i-j), dp[i])
        return dp[-1]
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/integer-break
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

# 完全平方数

动态规划， 与前一题类似

```java
import java.lang.Math;
class Solution {
    public int numSquares(int n) {
        if(n<=3) return n;
        int[] dp = new int[n+1];
        for(int i=0;i<n+1; i++){
            dp[i] = Integer.MAX_VALUE;
        }
        for(int i=1; i<=3;i++){
            dp[i] = i;
        }
        for(int i=4; i<n+1; i++){
            int max_n = (int)Math.sqrt(i);
            if(max_n*max_n==i){
                dp[i]=1;
            } else{
                for(int j=1; j<=max_n; j++){
                    dp[i] = Math.min(dp[i], dp[i-j*j]+dp[j*j]);
                }
            }
        }
        return dp[n];
    }
}
```

```python
class Solution(object):
    def numSquares(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n <= 3:
            return n
        dp = [sys.maxsize] * (n + 1)
        for i in range(1, 4):
            dp[i] = i
        for i in range(4, n + 1):
            max_n = int(math.sqrt(i))
            if max_n * max_n == i:
                dp[i] = 1
            else:
                for j in range(1, max_n + 1):
                    dp[i] = min(dp[i], dp[i - j * j] + dp[j*j])
        return dp[-1]
```

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var numSquares = function(n) {
    if(n<=3) return n;
    let dp = new Array(n+1).fill(Number.MAX_VALUE);
    for(let i=1; i<=3; i++){
        dp[i] = i;
    }
    for(let i=4; i<n+1; i++){
        max_n = Math.trunc(Math.sqrt(i));
        if(max_n*max_n==i) dp[i]=1;
        else{
            for(let j=1; j<=max_n; j++){
                dp[i] = Math.min(dp[i], dp[i-j*j]+dp[j*j]);
            }
        } 
    }
    return dp[n];
};
```

# 解码方法

一条包含字母 A-Z 的消息通过以下方式进行了编码：

'A' -> 1
'B' -> 2
...
'Z' -> 26
给定一个只包含数字的非空字符串，请计算解码方法的总数。

示例 1:

输入: "12"
输出: 2
解释: 它可以解码为 "AB"（1 2）或者 "L"（12）。
示例 2:

输入: "226"
输出: 3
解释: 它可以解码为 "BZ" (2 26), "VF" (22 6), 或者 "BBF" (2 2 6) 。

**思路：**动态规划，需注意0的处理，1010的编码方式共有1种，而909编码方式为0种，202编码方式为1种。

```python
class Solution(object):
    def numDecodings(self, s):
        """
        :type s: str
        :rtype: int
        """
        l = len(s)
        if l==0 or s[0]=='0': return 0
        dp = [0]*(l+1)
        dp[0], dp[1] = 1, 1
        for i in range(2, l+1):
            if s[i-1]!='0':
                dp[i] += dp[i-1]
            if 9<int(s[i-2:i])<=26:
                dp[i]+=dp[i-2]
        return dp[-1]      
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/decode-ways
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。