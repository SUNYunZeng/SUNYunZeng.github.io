---
title: Algortthm--Jewels and Stones
toc: true
date: 2019-05-19 10:57:16
categories: 算法题
tags: LeetCode
---

# **Map的应用**

## **Jewels and Stones**

**Title Detail**

>You're given strings J representing the types of stones that are jewels, and S representing the stones you have.  Each character in S is a type of stone you have.  You want to know how many of the stones you have are also jewels.

>The letters in J are guaranteed distinct, and all characters in J and S are letters. Letters are case sensitive, so "a" is considered a different type of stone from "A".

>**Example 1:**
```Java
Input: J = "aA", S = "aAAbbbb"
Output: 3
```
>**Example 2:**
```Java
Input: J = "z", S = "ZZ"
Output: 0
```
>**Note:**

>S and J will consist of letters and have length at most 50.
>The characters in J are distinct.

### 思路

使用Java里面的 **`Map`**。key键保存宝石类型，value键保存数量统计。

### Algorithm

```Java
class Solution {
    public int numJewelsInStones(String J, String S) {
        Map<Character, Integer> map = new HashMap<Character, Integer>();
        for(int i=0; i<J.length(); i++){
            map.put(J.charAt(i), 0);
        }
        for(int i=0; i<S.length();i++){
            Character c = S.charAt(i);
            if(map.get(c)!=null){
                Integer freq =map.get(c);
                map.put(c, freq+1);
            }
        }
        int result = 0;
        for(Integer value: map.values()){
            result += value;
        }
        return result;
    }
}
```

### **法2 正则表达式**

```Java
String  a = "[^"+J+"]";
        S = S.replaceAll(a, "");
        return S.length();
```



### **Python方法**

```Python
return sum(S.count(i) for i in J)
```

[题目链接](https://leetcode-cn.com/problems/jewels-and-stones/)