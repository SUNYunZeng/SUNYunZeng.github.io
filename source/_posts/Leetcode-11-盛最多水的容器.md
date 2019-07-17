---
title: Leetcode 11.盛最多水的容器
comments: true
toc: true
date: 2019-07-17 15:08:09
categories: Algorithm
tags: Other Algorithm
---

# 题目

给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

**说明:** 你不能倾斜容器，且 n 的值至少为 2。
{% asset_img question_11.jpg %}

# 思路 + 代码

**方法1：暴力破解法**

即遍历所有情况，找到最优解。

```java
class Solution {
    public int maxArea(int[] height) {
        int len = height.length;
        int res = 0;
        for(int i=1; i<len; i++){
            for(int j=0; j<i; j++){
                res = Math.max((i-j)*Math.min(height[i], height[j]),res);
            }
        }
        return res;
    }
}
```
时间复杂度O(n^2), 空间复杂度 1

**方法2：双指针法**

两个指针，一个指向数组首，一个指向数组尾。

指向数字小的往中间移动，并计算一次结果。

取最优即为最终结果。

原因[请戳](https://leetcode.com/problems/container-with-most-water/discuss/6099/Yet-another-way-to-see-what-happens-in-the-O(n)-algorithm)

```java
class Solution {
    public int maxArea(int[] height) {
        int last = height.length-1;
        int first = 0;
        int res = 0;
        while(first < last){
            int tmp = (last-first)*Math.min(height[first], height[last]);
            if(height[first] <= height[last]){
                first++;
            }else{
                last--;
            }
            res = Math.max(res,tmp);
        }
        return res;
    }
}
```

时间复杂度O(n), 空间复杂度 1 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/container-with-most-water
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。