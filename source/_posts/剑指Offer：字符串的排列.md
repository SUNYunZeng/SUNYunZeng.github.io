---
title: 剑指Offer：字符串的排列
comments: true
toc: true
date: 2019-10-10 18:25:16
categories: 算法题
tags: 剑指Offer
---

# 题目

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

**输入描述：**

输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。

# 思路 + 代码

典型回溯法，同 [全排列]() 解法类似。

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<String> Permutation(String str) {
       ArrayList<String> res = new ArrayList<String>();
       backtracking(res, str.toCharArray(), new StringBuilder(), new boolean[str.length()]);
       return res;
    }
    private void backtracking(ArrayList<String> res, char[] chs, StringBuilder sbd, boolean[] used){
        if(chs==null || chs.length==0)
            return;
        else{
            if(chs.length==sbd.length()){
                res.add(new String(sbd.toString()));
            }else{
                for(int i=0; i<chs.length; i++){
                    if(used[i] || i>0&&chs[i]==chs[i-1]&&!used[i-1])
                        continue;
                    sbd.append(chs[i]);
                    used[i] = true;
                    backtracking(res, chs, sbd, used);
                    sbd.deleteCharAt(sbd.length()-1);
                    used[i] = false;
                }
            }
        }
    }
}
```

<u><font color=#1772b4>[题目链接](https://www.nowcoder.com/practice/fe6b651b66ae47d7acce78ffdd9a96c7?tpId=13&tqId=11180&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)</font></u>