---
title: Algorithm--Third Maximum Number
toc: true
date: 2019-05-23 13:37:55
categories: 算法题
tags: LeetCode
---

# 1. Third Maximum Number

## Title Detail
>Given a non-empty array of integers, return the third maximum number in this array. If it does not exist, return the maximum number. **The time complexity must be in O(n).**

>**Example 1:**

```Java
Input: [3, 2, 1]
Output: 1
Explanation: The third maximum is 1.
```

>**Example 2:**
```Java
Input: [1, 2]
Output: 2
Explanation: The third maximum does not exist, so the maximum (2) is returned instead.
```

>**Example 3:**

```Java
Input: [2, 2, 3, 1]
Output: 1
```
>**Explanation:**
    >Note that the third maximum here means the third maximum distinct number.
    >Both numbers with value 2 are both considered as second maximum.

## **思路**

因为要求算法的时间复杂度为O(n)，所以不能用排序的方法。因为只求第三大的数，跟求第一大的数原理类似，只不过注意边界条件。

## **Algorithm**

```Java
class Solution {
    public int thirdMax(int[] nums) {
        if(nums.length==1) return nums[0];
        if(nums.length==2) return Math.max(nums[0], nums[1]);
        int max1 = Integer.MIN_VALUE;
        int max2 = Integer.MIN_VALUE;
        int max3 = Integer.MIN_VALUE;
        int flag = 0;
        boolean f = true;
        for(int i=0; i<nums.length; i++){
            if(nums[i]>max1){
                max3=max2;
                max2=max1;
                max1=nums[i];
                ++flag;
                continue;
            }
            if(max1>nums[i] && nums[i]>max2){
                max3=max2;
                max2=nums[i];
                ++flag;
                continue;
            }
            if(max2>nums[i] && nums[i]>max3){
                max3=nums[i];
                ++flag;
                continue;
            }
            // 边界条件，如果有多个最小值，则只算一次
            if(nums[i]==Integer.MIN_VALUE&&f){
                flag++;
                f = false;
                continue;
            }
        }
        return (flag<3?max1:max3);
    }
}
```
[题目链接](https://leetcode-cn.com/problems/third-maximum-number/)

# 2. Smallest Range I

## Title Detail 

>Given an array A of integers, for each integer A[i] we may choose any x with -K <= x <= K, and add x to A[i].
After this process, we have some array B.
Return the smallest possible difference between the maximum value of B and the minimum value of B.

**Example 1:**
```Java
Input: A = [1], K = 0
Output: 0
Explanation: B = [1]
```

**Example 2:**
```Java
Input: A = [0,10], K = 2
Output: 6
Explanation: B = [2,8]
```

**Example 3:**
```Java
Input: A = [1,3,6], K = 3
Output: 0
Explanation: B = [3,3,3] or B = [4,4,4]
```

**Note:**
```Java
1 <= A.length <= 10000
0 <= A[i] <= 10000
0 <= K <= 10000
```

## 思路 
求出数组最大值与最小值的差值，比较差值与K/2的关系。

## Algorithm

```Java
class Solution {
    public int smallestRangeI(int[] A, int K) {
        if(A.length==0) 
            return 0;
        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;
        for(int i=0; i<A.length; i++){
            if(A[i]<min) min = A[i];
            if(A[i]>=max) max = A[i];
        }
        int len = max-min;
        if(len>2*K) 
            return len-2*K;
        else 
            return 0;
    }
}
```
[题目链接](https://leetcode-cn.com/problems/smallest-range-i/)

# 3. 数组形式的整数加法

## 题目细节

>对于非负整数 X 而言，X 的数组形式是每位数字按从左到右的顺序形成的数组。例如，如果 X = 1231，那么其数组形式为 [1,2,3,1]。

>给定非负整数 X 的数组形式 A，返回整数 X+K 的数组形式。

**示例1：**
```Java
输入：A = [1,2,0,0], K = 34
输出：[1,2,3,4]
解释：1200 + 34 = 1234
```

**示例2：**
```Java
输入：A = [2,7,4], K = 181
输出：[4,5,5]
解释：274 + 181 = 455
```

**示例3：**
```Java
输入：A = [2,1,5], K = 806
输出：[1,0,2,1]
解释：215 + 806 = 1021
```

**示例4：**
```Java
输入：A = [9,9,9,9,9,9,9,9,9,9], K = 1
输出：[1,0,0,0,0,0,0,0,0,0,0]
解释：9999999999 + 1 = 10000000000
```

**提示：**
```Java
1 <= A.length <= 10000
0 <= A[i] <= 9
0 <= K <= 10000
如果 A.length > 1，那么 A[0] != 0
```

## 思路

最初思路是字符串转数字，相加后转字符串。

但是最终没过，因为数据量太大，不允许。

后又通过进位方式实现，老太太的裹脚布。

## Algorithm

```Java
class Solution {
    public List<Integer> addToArrayForm(int[] A, int K) {
        int len = A.length;
        for(int i=len-1; i>=0; --i) {
            int temp = A[i] + K;
            if (temp >= 10) {
                A[i] = temp % 10;
                K = temp / 10;
            } else {
                A[i] = temp;
                K = temp /10;
                break;
            }
        }
        List<Integer> result = new ArrayList<>();
        if (K!=0){
            String s = String.valueOf(K);
            for(char c: s.toCharArray())
            result.add(c-'0');
        }
        for (int i : A) {
            result.add(i);
        }
        return result;
    }
}
```

>官方答案

```Java
class Solution {
    public List<Integer> addToArrayForm(int[] A, int K) {
        int N = A.length;
        int cur = K;
        List<Integer> ans = new ArrayList();

        int i = N;
        while (--i >= 0 || cur > 0) {
            if (i >= 0)
                cur += A[i];
            ans.add(cur % 10);
            cur /= 10;
        }

        Collections.reverse(ans);
        return ans;
    }
}
```

[题目链接](https://leetcode-cn.com/problems/add-to-array-form-of-integer/)