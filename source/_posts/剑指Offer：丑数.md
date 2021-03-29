---
title: 剑指Offer：丑数
comments: true
toc: true
date: 2019-10-15 18:25:13
categories: 算法题
tags: 剑指Offer
---

# 题目

把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

#　思路　＋　代码

丑数只能是丑数乘以　**2、３、５**。

按照顺序存储丑数，也就是比较每次丑数的大小。

```java
public class Solution {
    public int GetUglyNumber_Solution(int index) {
        if(index <= 0)
            return 0;
        int[] dp = new int[index];
        dp[0] = 1;
        int i2 = 0, i3 = 0, i5 = 0;
        for(int i=1; i<index; i++){
            dp[i] = Math.min(Math.min(dp[i2]*2, dp[i3]*3), dp[i5]*5);
            //-----here is three "if" judge，rather tha “if else”-----
            if(dp[i] == dp[i2]*2)
                i2++;
            if(dp[i] == dp[i3]*3)
                i3++;
            if(dp[i] == dp[i5]*5)
                i5++;
        }
        return dp[index-1];
    }
}
```
