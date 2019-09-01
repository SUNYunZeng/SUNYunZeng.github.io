---
title: Leetcode 无重复字符的最长子串（String 练习 02）
comments: true
toc: true
date: 2019-08-30 21:23:46
categories: Algorithm
tags: Other Algorithm
---

# 题目

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

**示例1:**
```java
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例2:**
```java
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例3:**
```java
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

# 思路 1 

暴力法，用Set记录检查的无重复的最长子串。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        char[] chars = s.toCharArray();
        if(chars.length==1) return 1;
        Set<Character> set = new HashSet<>();
        int result = 0;
        for(int i=0; i < chars.length-1; i++){      
            set.add(chars[i]);
            for(int j=i+1; j<chars.length;j++){
                if(!set.contains(chars[j])){
                    set.add(chars[j]);
                }else{
                    break;
                }
            }
            result = Math.max(result, set.size());
            set.clear();
        }
        return result;
    }
}
```

# 思路 2 

滑动窗口法。

暴力法虽然容易想到，但是很多情况重复考虑了。例如假定在 i ~ j 子串为不重复子串，那么该子串内的子串都会不重复。

滑动窗口法 1：采用标记记录左侧窗口的索引值。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int result = 0;
        char[] chars = s.toCharArray();
        int leftIndex = 0;
        // 滑动窗口的右边索引
        for(int i=0; i<chars.length; i++){
            // 对于当前的滑动窗口进行重复性字符检查
            for(int checkIndex = leftIndex; checkIndex<i; checkIndex++){
                // 如果存在相同字符串，就更新窗口左边索引
                if(chars[checkIndex]==chars[i]){
                    // 更新结果
                    result = Math.max(result, i-leftIndex);
                    leftIndex = checkIndex+1;
                    break;a
                }
            }
        }
        // 检查最后一次窗口的长度与result保留长度进行对比
        return Math.max(chars.length-leftIndex, result);
    }
}
```

滑动窗口 2：巧用 **HashSet**，利用HashSet维护范围为 **[i,j)** 的滑动窗口。

先滑动右边，j++。如果存在重复，记录此时长度，再滑动左边 i++。直到所有的 i 遍历完成。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int ans=0, i=0, j=0;
        int len = s.length();
        Set<Character> set = new HashSet<>();
        while(i < len && j < len){
            if(!set.contains(s.charAt(j))){
                set.add(s.charAt(j++));
                ans = Math.max(ans, j-i);
            }else{
                set.remove(s.charAt(i++));
            }
        }
        return ans;
    }
}
```

滑动窗口 3：上面的优化。如果 j 存在重复，那么 i 不仅移动一位，而是移动到 j+1 的位置。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> map = new HashMap<>();
        int ans = 0;
        int len = s.length();
        for(int i=0, j=0; j<len; j++){
            if(map.containsKey(s.charAt(j))){
                i = Math.max(i, map.get(s.charAt(j)));
            }
            ans = Math.max(ans, j-i+1);
            map.put(s.charAt(j), j+1);
        }
        return ans;
    }
}
```