---
title: 剑指Offer：扑克牌顺子
comments: true
toc: true
date: 2019-10-23 12:54:32
categories: 算法题
tags: 剑指Offer
---

# 题目

LL今天心情特别好,因为他去买了一副扑克牌,发现里面居然有2个大王,2个小王(一副牌原本是54张^_^)...他随机从中抽出了5张牌,想测测自己的手气,看看能不能抽到顺子,如果抽到的话,他决定去买体育彩票,嘿嘿！！“红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子.....LL不高兴了,他想了想,决定大\小 王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。LL决定去买体育彩票啦。 现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何， 如果牌能组成顺子就输出true，否则就输出false。为了方便起见,你可以认为大小王是0。

# 思路 + 代码

首先用一个长度为13的数组统计每个数字出现的次数。

然后满足以下条件为顺子：

1. 除大小王外所有数字出现一次。
2. 最大值与最小值差值小于等于4，例如存在 1 跟 6 无法形成顺子。
3. 四个癞子稳赢。

```java
public class Solution {
    public boolean isContinuous(int [] numbers) {
        if(numbers==null || numbers.length==0)
            return false;
        int[] counts = new int[14];
        for(int num: numbers)
            counts[num]++;
        int n = 0;
        int i=0, j=14;
        //四个癞子稳赢
        if(counts[0]==4)
            return true;
        // 寻找最大值与最小值
        while(counts[++i]==0);
        while(counts[--j]==0);
        // 例如：最小值1，最大值6肯定不行
        if(Math.abs(j-i)>4)
            return false;
        // 用癞子补全，如果癞子够，就可以
        for(int k=i; k<=j; k++)
            if(counts[k]>1)
                return false;
        return true;
    }
}
```

**其它方法：**

```java
import java.util.*;
public class Solution {
    public boolean isContinuous(int [] numbers) {
        if(numbers==null || numbers.length<5)
            return false;
        int count = 0;
        Arrays.sort(numbers);
        //统计癞子数量
        for(int num: numbers){
            if(num==0)
                count++;
            else
                break;
        }
        for(int i=count; i<4; i++){
            // 存在重复的
            if(numbers[i]==numbers[i+1])
                return false;
            count -= numbers[i+1]-numbers[i]-1;
        }
        return count>=0;
    }
}
```