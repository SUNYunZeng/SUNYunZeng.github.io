---
title: 剑指Offer：整数中1出现的次数
comments: true
toc: true
date: 2019-10-14 20:48:41
categories: Algorithm
tags: 剑指Offer
---

# 题目

求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数（从1 到 n 中1出现的次数）。

# 思路 + 代码

暴力穷举会超出时间，这道题的关键是找规律。

<center>{% asset_img number_of_digit_one.png %}</center>
<center>[图片来自LeetCode 233](https://leetcode-cn.com/problems/number-of-digit-one/solution/shu-zi-1-de-ge-shu-by-leetcode/)</center>

一个数字，分解为十位、百位、千位...上来看。

因为数字由1~n组成，既有十位、百位、千位等组成。

**首先，每十个数，个位数字出现一次。每百位数，个位数字出现十次...**

**`n/(i*10)*i, i=1, 10, 100, ...`**

**其次，对于后面的数，例如1611，从百位数来看，前面的数字 1611/100*10=160, 后面数字为11，因此后面的个位数为 1+1=2， 而1620和1650后面的个位数字都是10个，因此后面个位数字：**

**`max(min(n%(i*10)-i+1,0),i)`**

```java
import java.lang.Math;
public class Solution {
    public int NumberOf1Between1AndN_Solution(int n) {
        int count = 0;
         for(int i=1; i<=n; i*=10){
             count += n/(i*10)*i + Math.min(Math.max(n%(i*10)-i+1,0),i);
         }
        return count;
    }
}
```

