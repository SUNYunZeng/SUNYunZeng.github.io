---
title: 剑指Offer：翻转单词顺序列
comments: true
toc: true
date: 2019-10-22 22:20:34
categories: Algorithm
tags: 剑指Offer
---

# 题目

牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

# 思路 + 代码

先翻转每个单词，再重头到尾进行翻转。

```java
public class Solution {
    public String ReverseSentence(String str) {
        if(str==null || str.length()==0)
            return str;
        char[] chs = str.toCharArray();
        int len = chs.length;
        int i=0, j=0;
        while(j<=len){
            // 反转单词
            if(j==len || chs[j]==' '){
                reverse(chs, i, j-1);
                i=j+1;
            }
            j++;
        }
        // 反转整个句子
        reverse(chs, 0, len-1);
        return new String(chs);
    }
    private void reverse(char[] chs, int i, int j){
        while(i<j)
            swap(chs, i++, j--);
    }
    private void swap(char[] chs, int i, int j){
        char t = chs[i];
        chs[i] = chs[j];
        chs[j] = t;
    }
}
```
