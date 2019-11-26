---
title: LeetCode 75.颜色分类
comments: true
toc: true
date: 2019-11-26 19:34:45
categories: Algorithm
tags: Other Algorithm
---

# 题目

给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

注意:
不能使用代码库中的排序函数来解决这道题。

示例:
```java
输入: [2,0,2,1,1,0]
输出: [0,0,1,1,2,2]
```

# 思路 + 代码

先统计0,1,2分别出现的次数，然后按照各自数量依次赋予数组新数值。

时间复杂度: O(N^2)
空间复杂度: O(1)

```java
class Solution {
    public void sortColors(int[] nums) {
        if(nums==null || nums.length==0) return;
        int[] counts = new int[3];
        for(int i=0; i<nums.length; i++){
            counts[nums[i]]++;
        }
        for(int i=0;i<nums.length; i++){
            if(counts[0]>0){
                nums[i] = 0;
                counts[0]--;
            }else if(counts[1]>0){
                nums[i] = 1;
                counts[1]--;
            }else{
                nums[i] = 2;
            }
        }
    }
}
```
或者三指针方法，左指针负责交换0， 右指针负责交换2，中间指针负责扫描。
时间复杂度: O(N)
空间复杂度: O(1)

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        p0 = cur = 0
        p1 = len(nums) - 1
        while cur <= p1:
            if nums[cur] == 0:
                nums[cur], nums[p0] = nums[p0], nums[cur]
                # 这里左边遍历当前位置加一，因为左边的确定全部扫描过，而右边交换过的未进行判断
                cur += 1
                p0 += 1
            elif nums[cur] == 1:
                cur +=1
            else:
                nums[cur], nums[p1] = nums[p1], nums[cur]
                p1 -=1
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sort-colors
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
