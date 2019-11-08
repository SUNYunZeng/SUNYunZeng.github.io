---
title: LeetCode 238.除自身以外数组的乘积
comments: true
toc: true
date: 2019-11-07 22:35:22
categories: Algorithm
tags: Other Algorithm
---

# 题目

给定长度为 n 的整数数组 nums，其中 n > 1，返回输出数组 output ，其中 output[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积。

**示例：**
```java
输入: [1,2,3,4]
输出: [24,12,8,6]
```

**说明：**请不要使用除法，且在 O(n) 时间复杂度内完成此题。

# 思路 + 代码

不能常规的循环暴力解决，因为时间限制在O(n)内。

考虑上三角/下三角的乘法。

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int len = nums.length;
        int[] res = new int[len];
        Arrays.fill(res, 1);
        int left = 1, right=1;
        for(int i=0; i<len; i++) {
            // 只经过左指针操作，res[0] = 1, res[1] = 1, res[2] = 1*2, res[3] = 1*2*3
            res[i] *= left;
            left *= nums[i];
            // 只经过右指针操作，res[0] = 2*3*4, res[1] = 3*4, res[2] = 4 res[3] = 1
            res[len-1-i] *= right;
            right *= nums[len-1-i];
            // 上下相乘得到结果
        }
        return res;
    }
}
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/product-of-array-except-self
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
