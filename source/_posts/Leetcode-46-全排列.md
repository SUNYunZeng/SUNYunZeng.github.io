---
title: Leetcode 46.全排列
comments: true
toc: true
date: 2019-10-10 15:47:46
categories: 算法题
tags: 回溯法
---

# 题目

给定一个没有重复数字的序列，返回其所有可能的全排列。

**示例：**
```java
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

# 思路 + 代码

回溯问题。

如果遇到一个问题，不用穷举所有情况找不出答案，就采用回溯。

回溯是DFS的一种，遍历完一条路径后，回退一步，继续寻找下一可能路径。

它与暴力法的区别在于，可以通过剪枝规避掉很多无意义的尝试，且不用每次都从头开始遍历到尾部。

讲解可以参考<u>https://www.cis.upenn.edu/~matuszek/cit594-2012/Pages/backtracking.html</u>

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        backtracking(res, new ArrayList<Integer>(), nums, new boolean [nums.length]);
        return res;
    }
    // used访问标志防止重复使用
    private void backtracking(List<List<Integer>> res, ArrayList<Integer> arr, int[] nums, boolean[] used){
        if(nums==null || nums.length==0){
            res.add(arr);
            return;
        }
        if(arr.size()==nums.length){
            res.add(new ArrayList<Integer>(arr));
            return;
        }else{
            for(int i=0; i<nums.length; i++){
                if(used[i])
                    continue;
                arr.add(nums[i]);
                used[i] = true;
                backtracking(res, arr, nums, used);
                arr.remove(arr.size()-1);
                used[i] = false;
            }
        }
    }
}
```

**<font color=#f07c82>[回溯例题参考](https://blog.csdn.net/wonner_/article/details/80373871)</font>**

来源：力扣（LeetCode）
链接：<u>https://leetcode-cn.com/problems/permutations</u>
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。