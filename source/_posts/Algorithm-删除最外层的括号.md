---
title: Algorithm：删除最外层的括号
toc: true
date: 2019-05-21 21:42:55
categories: Algorithm
tags: Other Algorithm
---

# 删除最外层括号

## 题目

>有效括号字符串为空 ("")、"(" + A + ")" 或 A + B，其中 A 和 B 都是有效的括号字符串，+ 代表字符串的连接。例如，""，"()"，"(())()" 和 "(()(()))" 都是有效的括号字符串。

>如果有效字符串 S 非空，且不存在将其拆分为 S = A+B 的方法，我们称其为**原语（primitive）**，其中 A 和 B 都是非空有效括号字符串。

>给出一个非空有效字符串 S，考虑将其进行原语化分解，使得：S = P_1 + P_2 + ... + P_k，其中 P_i 是有效括号字符串原语。

>对 S 进行原语化分解，删除分解中每个原语字符串的最外层括号，返回 S 。

**示例1**

>输入："(()())(())"
输出："()()()"
解释：
输入字符串为 "(()())(())"，原语化分解得到 "(()())" + "(())"，
删除每个部分中的最外层括号后得到 "()()" + "()" = "()()()"。

**示例2**

>输入："(()())(())(()(()))"
输出："()()()()(())"
解释：
输入字符串为 "(()())(())(()(()))"，原语化分解得到 "(()())" + "(())" + "(()(()))"，
删除每隔部分中的最外层括号后得到 "()()" + "()" + "()(())" = "()()()()(())"。

**示例3**

>输入："()()"
输出：""
解释：
输入字符串为 "()()"，原语化分解得到 "()" + "()"，
删除每个部分中的最外层括号后得到 "" + "" = ""。

**提示**

```Java
S.length <= 10000
S[i] 为 "(" 或 ")"
S 是一个有效括号字符串
```

## 思路

用一个数组模拟栈，发现 "(" 使其进栈，发现 ")" 时使 "(" 出栈。

**注意:** 栈底的 "(" 出栈不输出，当输出字符串中 "(" 和 ")" 数量不相等时，才输出 ")" 。

## 代码

```Java
class Solution {
    public String removeOuterParentheses(String S) {
        // list作为栈，记录(数量
        List<Character> list = new ArrayList();
        StringBuilder bf = new StringBuilder();
        int left = 0; // 记录输出的 ( 数量
        int right = 0; //记录输出的 ) 数量
        for(int i=0; i<S.length(); i++){
            char c = S.charAt(i);
            if(c=='(') list.add(c);
            if(c==')'){
                if(list.size()==1&&left==right) list.remove(0);
                if(list.size()>1){
                    while(list.size()!=1){
                        bf.append("(");
                        list.remove(list.size()-1);
                        ++left;
                    }
                }
                if(left!=right){
                    bf.append(")");
                    ++right;
                }
            }
        }
        return bf.toString();
    }
}
```

## 更**Niubility**的code：

```Java
class Solution {
    public String removeOuterParentheses(String S) {
        StringBuilder bf = new StringBuilder();
        int flag = 0;
        for(char c: S.toCharArray()){
            if(c == '('){
                ++flag;
                if(flag == 1) continue;
            }
            if(c==')'){
                --flag;
                if(flag == 0) continue;
            }
            bf.append(c);
        }
        return bf.toString();
    }
}
```