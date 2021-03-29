---
title: LeetCode 494.目标和
comments: true
toc: true
date: 2019-11-04 21:54:31
categories: 算法题
tags: LeetCode
---

# 题目

给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 + 和 -。对于数组中的任意一个整数，你都可以从 + 或 -中选择一个符号添加在前面。

返回可以使最终数组和为目标数 S 的所有添加符号的方法数。

**示例1：**
```java
输入: nums: [1, 1, 1, 1, 1], S: 3
输出: 5
解释: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

一共有5种方法让最终目标和为3。
```

**注意：**
1. 数组非空，且长度不会超过20。
2. 初始的数组的和不会超过1000。
3. 保证返回的最终结果能被32位整数存下。

# 思路 + 代码

**首先是回溯方法。**

```java
class Solution {
    private int cnt = 0;
    public int findTargetSumWays(int[] nums, int S) {
        dfs(nums, S, 0, 0);
        return cnt;
    }
    private void dfs(int[]nums, int S, int tmpS, int location){
        if(location==nums.length){
            if(S==tmpS)
                cnt++;
            return;
        }
        dfs(nums, S, tmpS + nums[location], location+1);
        dfs(nums, S, tmpS - nums[location], location+1);
    }
}
```

**然后是动态规划**

一个数组可分为两个子集和 X 跟 Y，然后 X-Y=S，X+Y=Sum，由此得到 X=(Sum+S)/2。

也就是寻找一个子集和X，满足 X=(Sum+S)/2，此和且为整数。

```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
       int sum = 0;
       for(int n: nums){
           sum += n;
       }
       if(sum < S || (sum+S)%2!=0){
           return 0;
       }
       int s = (sum+S)/2;
       int[] dp = new dp[s+1];
       dp[0] = 1;
       for(int n: nums){
           for(int i=s; i>=n; i--){
               // 如果考虑数n，结果数为dp[i]，否则为dp[i-n]
               dp[i] += dp[i-n];
           }
       }
       return dp[s];
    }
}
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/target-sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。