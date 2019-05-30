---
title: Algorithm--Sqrt(x)
toc: true
mathjax: true
date: 2019-05-19 13:11:49
categories: Algorithm
tags: Other Algorithm
---

# Some Algorithm using Math Algorithm

## **Algorithm-1 Sqrt(x)**

**Title detail**

>Implement int sqrt(int x)
>Compute and return the square root of x, where x is guaranteed to be a non-negative integer.

>Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.

>**Example 1:**
```Java
Input: 4
Output: 2
```
>**Example 2:**
```Java
Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since 
             the decimal part is truncated, 2 is returned.
```
### **思路**

[牛顿迭代法](https://en.wikipedia.org/wiki/Integer_square_root#Using_only_integer_division)

>公式： $x_{k+1} = \lfloor {x_k +  n / {x_k}} \rfloor / 2$

>当$k \rightarrow \infty$ 时，${x_{k+1}}^2 \rightarrow  n$

### **Algorithm**

```Java
class Solution {
    public int mySqrt(int x) {
        if(x<=1) return x;
        long r=x;
        long t = x/r;
        while(r>t){
            t = x/r;
            r = (r + t)/2;
        }
        return (int)r;
    }
}
```
[题目链接](https://leetcode-cn.com/problems/sqrtx/)