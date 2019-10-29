---
title: 剑指Offer：字符流中第一个不重复的字符
comments: true
toc: true
date: 2019-10-26 22:25:16
categories: Algorithm
tags: Other Algorithm
---

# 题目

请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。

如果当前字符流没有存在出现一次的字符，返回#字符。

# 思路 + 代码

利用队列进行结果的存储，利用一个数组进行出现次数的统计。

队列对于其中所有出现超过两次的字符进行出栈。

```java
import java.util.*;
public class Solution {
    private char[] tmp = new char[256];
    private Queue<Character> queue = new LinkedList<Character>();
    //Insert one char from stringstream
    public void Insert(char ch)
    {
        tmp[ch]++;
        queue.add(ch);
        while(!queue.isEmpty()&&tmp[queue.peek()]>1)
            queue.poll();
    }
  //return the first appearence once char in current stringstream
    public char FirstAppearingOnce()
    {
        return queue.isEmpty()?'#':queue.peek();
    }
}
```