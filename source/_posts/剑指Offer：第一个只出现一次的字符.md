---
title: 剑指Offer：第一个只出现一次的字符
comments: true
toc: true
date: 2019-10-15 18:45:02
categories: Algorithm
tags: 剑指Offer
---

# 题目

在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.

# 思路 + 代码

用一个LinkedHashMap统计每个字符出现的次数，同时保存顺序。然后String查找该字符第一次出现的位置。

```java
import java.util.*;
public class Solution {
    public int FirstNotRepeatingChar(String str) {
        if(str==null || str.length()==0)
            return -1;
        char[] chs = str.toCharArray();
        LinkedHashMap<Character, Integer> map = new LinkedHashMap<Character, Integer>();
        int len = chs.length;
        for(int i=0; i<len; i++){
            if(!map.containsKey(chs[i])){
                map.put(chs[i], 1);
            }else{
                map.put(chs[i], map.get(chs[i])+1);
            }
        }
        for(Map.Entry<Character, Integer> entry: map.entrySet()){
            if(entry.getValue()==1)
                return str.indexOf(entry.getKey());
        }
        return -1;
    }
}
```

或者可以用一个数组来模拟Map。

```java
import java.util.*;
public class Solution {
    public int FirstNotRepeatingChar(String str) {
        if(str==null || str.length()==0)
            return -1;
        int[] counts = new int[256];
        int len = str.length();
        for(int i=0; i<len; i++){
            counts[str.charAt(i)]++;
        }
        for(int i=0; i<len; i++){
            if(counts[str.charAt(i)]==1)
                return i;
        }
        return -1;
    }
}
```

空间复杂度可以继续优化，用两个Bitset分别表示出现一次和出现一次及以上。

```java
import java.util.*;
public class Solution {
    public int FirstNotRepeatingChar(String str) {
        if(str==null || str.length()==0)
            return -1;
        int len = str.length();
        char[] chs = str.toCharArray();
        BitSet bs1 = new BitSet(256);
        BitSet bs2 = new BitSet(256);
        for(char c: chs){
            if(!bs1.get(c) && !bs2.get(c)){
                // 出现一次
                bs1.set(c);
            }else if(bs1.get(c) && !bs2.get(c)){
                // 出现一次以上
                bs2.set(c);
            }
        }
        
        for(int i=0; i<len; i++){
            char c = str.charAt(i);
            if(bs1.get(c) && !bs2.get(c))
                return i;
        }
       
        return -1;
    }
}
```