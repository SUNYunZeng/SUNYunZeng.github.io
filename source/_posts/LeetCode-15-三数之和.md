---
title: LeetCode 15.三数之和
comments: true
toc: true
date: 2019-09-24 09:27:20
categories: Algorithm
tags: Other Algorithm
---

# 题目

给定一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？找出所有满足条件且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

```java
例如, 给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

# 思路+代码

先<font color=#f07c82>排序</font>，然后三数之和等于零必定有<font color=#f07c82> 1 到 2 </font>个小于零，<font color=#f07c82> 1 到 2 </font>个大于零。

然后我们采用<font color=#f07c82>双指针</font>，先固定最小的一个数（负值）；然后双指针再后面区间寻找两个数，与前面数相加等于零。

<font color=#f07c82>注意：</font>因为不能包含重复三元组，在遍历过程中注意去重。<font color=#f07c82>保证每次遍历的数字与前面不一样即可</font>（因为排序了）。

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        // 排序求解
        Arrays.sort(nums);
        // 循环求解
        int len = nums.length;
        for(int i=0; i<len; i++){
            // 此时没有满足条件的情况
            if(nums[i]>0)
                break;
            // 避免重复计算
            if(i>0 && nums[i]==nums[i-1])
                continue;
            int j=i+1;
            int k=len-1;
            while(j<k){
                if(nums[j]+nums[k]==-nums[i]){
                    ArrayList<Integer> list = new ArrayList<Integer>();
                    list.add(nums[i]);
                    list.add(nums[j]);
                    list.add(nums[k]);
                    res.add(list);
                    // 跳过重复的数字
                    while(j<k&&nums[j]==nums[j+1]){
                        j++;
                    }
                    while(j<k&&nums[k]==nums[k-1]){
                        k--;
                    }
                    j++;
                    k--;
                }else if(nums[j]+nums[k]<-nums[i]){
                    // 左边负值大了
                    j++;
                    
                }else{
                    // 右边正值大了
                    k--;
                }
            }
        }
        return res;
    }
}
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/3sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。