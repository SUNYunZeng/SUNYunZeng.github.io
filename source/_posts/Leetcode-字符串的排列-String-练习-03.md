---
title: Leetcode 字符串的排列 (String 练习 03)
comments: true
toc: true
date: 2019-09-01 12:30:02
categories: Algorithm
tags: Other Algorithm
---

# 题目

给定两个字符串 s1 和 s2，写一个函数来判断 s2 是否包含 s1 的排列。

换句话说，第一个字符串的排列之一是第二个字符串的子串。


**示例1:**
```java
输入: s1 = "ab" s2 = "eidbaooo"
输出: True
解释: s2 包含 s1 的排列之一 ("ba").
```

**示例2:**
```java
输入: s1= "ab" s2 = "eidboaoo"
输出: False
```

**注意:**
```java
1. 输入的字符串只包含小写字母
2. 两个字符串的长度都在 [1, 10,000] 之间
```

# 思路 1

暴力法，滑动窗口依次判定。

但是**超出时间限制！**

```Java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int len1 = s1.length();
        int len2 = s2.length();
        if(len2<len1)
            return false;
        for(int i=0; i+len1<=len2; i++){
            if(checkSubString(s1, s2.substring(i, i+len1)))
                return true;
        }
        return false;
    }
    private boolean checkSubString(String subS1, String subS2){
        String s = new String(subS2);
        for(int i=0; i<subS1.length(); i++){
            if(s.indexOf(subS1.charAt(i))!=-1){
                s = s.replaceFirst(String.valueOf(subS1.charAt(i)), "");
            }
        }
        
        return s.isEmpty()?true:false;
    }
}
```

时间复杂度：O((len2-len1) * len1 * len2 * len2)

空间复杂度：O(1)

# 思路 2

也是滑动窗口法，不过不用内置的函数（使用过程中存在循环遍历），而利用数组存储各个字母出现的次数，进行子串是否匹配的判定依据。

```Java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int len1 = s1.length();
        int len2 = s2.length();
        if(len2<len1)
            return false;
        int[] temp1 = new int[26];
        for(int i=0; i<len1; i++)
            temp1[s1.charAt(i)-'a']++;
        for(int i=0; i+len1<=len2; i++){
            int[] temp2 = new int[26];
            for(int j=0; j<s1.length(); j++){
                temp2[s2.charAt(i+j)-'a']++;
            }
            if(match(temp1, temp2))
                return true;
        }
        return false;
    }
    private boolean match(int[] tmp1, int[] tmp2){
        for(int i=0; i<tmp1.length; i++){
            if(tmp1[i]!=tmp2[i])
                return false;
        }
        return true;
    }
}
```
时间复杂度：O(len1 + (len2-len1) * len1 * 26)

空间复杂度: O(1)

# 思路 3

基于思路2，继续进行优化。其实在滑动窗口中，每次只更新哈希表（数组）的第一个值及最后一个值，中间的不需要遍历。因此时间复杂度降低 len1

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int len1 = s1.length();
        int len2 = s2.length();
        if(len2<len1)
            return false;
        int[] temp1 = new int[26];
        int[] temp2 = new int[26];
        for(int i=0; i<len1; i++){
            temp1[s1.charAt(i)-'a']++;
            temp2[s2.charAt(i)-'a']++;
        }
        if(match(temp1, temp2))
            return true;
        for(int i=0; i+len1<len2; i++){
            temp2[s2.charAt(i+len1)-'a']++;
            temp2[s2.charAt(i)-'a']--;
            if(match(temp1, temp2))
                return true;
        }
        return false;
    }
    private boolean match(int[] tmp1, int[] tmp2){
        for(int i=0; i<tmp1.length; i++){
            if(tmp1[i]!=tmp2[i])
                return false;
        }
        return true;
    }
}
```

时间复杂度：O(len1 + (len2-len1) * 26)

空间复杂度: O(1)

作者：LeetCode
链接：https://leetcode-cn.com/problems/permutation-in-string/solution/zi-fu-chuan-de-pai-lie-by-leetcode/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。