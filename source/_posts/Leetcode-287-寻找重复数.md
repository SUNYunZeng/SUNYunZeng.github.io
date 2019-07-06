---
title: Leetcode 287.寻找重复数
comments: true
toc: true
date: 2019-07-06 19:55:07
categories: Algorithm
tags: Other Algorithm
---

# 题目

给定一个包含 n + 1 个整数的数组 nums，其数字都在 1 到 n 之间（包括 1 和 n），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

**示例1：**
```java
输入: [1,3,4,2,2]
输出: 2
```
**示例2：**
```java
输入: [3,1,3,4,2]
输出: 3
```
**说明：**
```
不能更改原数组（假设数组是只读的）。
只能使用额外的 O(1) 的空间。
时间复杂度小于 O(n2) 。
数组中只有一个重复的数字，但它可能不止重复出现一次。
```

# 思路 + 代码

**易想到方法**

不符合题目要求，因为需要额外的空间。

用链表或者HashMap存储数值，遇到相同的已存储的数就返回。空间复杂度最坏O(n)，时间复杂度最坏O(n)。

或者先排序，遇到相同的数返回。时间复杂度视排序方法而定，最好O(log(n))。


**巧妙算法1**

巧用快慢指针。

数组的索引与存储的数值之间形成了特殊**链表**。

如果存在重复的数，因为数组大小是 n+1，数字范围是1~n，所以该链表存在环。

环的入口即为结果。

答案的求解变成环入口的求解。[思路](https://blog.csdn.net/fynjy/article/details/47440049)

```java
class Solution {
    public int findDuplicate(int[] nums) {
        // 快慢指针
        int fast = nums[0];
        int low = nums[0];
        do{
            low = nums[low];
            fast = nums[nums[fast]];
        }while(fast != low);
        int step = nums[0];
        // 寻找环链表的入口，即为结果
        while(step != low){
            step = nums[step];
            low = nums[low];
        }
        return low;
    }
}
```

**巧妙算法2**

二分法。

统计小于中间数 mid 的数值数量，如果大于，则在0~mid之间寻找。

反之，则在mid~high中寻找。

最终找到重复的数值。

```java
class Solution {
    public int findDuplicate(int[] nums) {
        // 二分法
        int len = nums.length;
        int low = 0;
        int hight = len-1;
        while(low<hight){
            int count = 0;
            int mid = low+(hight-low)/2;
            for(int i=0; i<len; i++){
                if(nums[i]<=mid)
                    count++;
            }
            if(count<=mid)
                low = mid+1;
            else
                hight = mid;
        }
        return low;
    }
}
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/find-the-duplicate-number
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
