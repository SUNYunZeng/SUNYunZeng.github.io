---
title: Algorithm--Find Common Characters
toc: true
date: 2019-05-17 12:29:20
categories: 算法题
tags: LeetCode
---
# Find Common Characters
## **Title detail**

>Given an array A of strings made only from lowercase letters, return a list of all characters that show up in all strings within the list **(including duplicates)**.  For example, if a character occurs 3 times in all strings but not 4 times, you need to include that character three times in the final answer.

>You may return the answer in any order.

>**Example 1**
```Java
Input: ["bella","label","roller"]
Output: ["e","l","l"]
```
>**Example 2**
```Java
Input: ["cool","lock","cook"]
Output: ["c","o"]
```
>**Note:**
```Java
  1 <= A.length <= 100
  1 <= A[i].length <= 100
  A[i][j] is a lowercase letter
```
## 思路
统计每个字符串中26个字母的出现次数。每个字符串都出现的字母且统计次数最少的即为结果输出的字符。

用`int`数组0-25位置表示0-26个字母，存储的整数代表出现总次数。

## Algorithm

```Java
class Solution {
    public List<String> commonChars(String[] A) {
        List<String> list = new ArrayList();
        int[] res = new int[26];
        for(int i=0; i<A[0].length();i++){
            res[A[0].charAt(i) - 'a'] += 1;
        }
        for(int i=0; i<A.length;i++){
            int[] temp = new int[26];
            for(int j=0; j<A[i].length(); j++){
                temp[A[i].charAt(j) - 'a'] +=1;
            }
            for(int m=0; m<26; m++){
                res[m] = Math.min(res[m], temp[m]);
            }
        }
        for(int i=0; i<26; i++){
            if(res[i] > 0){
                for(int j=0; j<res[i]; j++){
                    list.add((char)('a'+i)+"");
                }
            }
        }
        return list;
    }
}
```

[题目链接](https://leetcode-cn.com/problems/find-common-characters/)