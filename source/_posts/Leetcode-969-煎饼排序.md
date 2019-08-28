---
title: Leetcode 969.煎饼排序
comments: true
toc: true
date: 2019-08-28 10:20:20
categories: Algorithm
tags: Other Algorithm
---

# 题目

给定数组 A，我们可以对其进行煎饼翻转：我们选择一些正整数 k <= A.length，然后反转 A 的前 k 个元素的顺序。我们要执行零次或多次煎饼翻转（按顺序一次接一次地进行）以完成对数组 A 的排序。

返回能使 A 排序的煎饼翻转操作所对应的 k 值序列。任何将数组排序且翻转次数在 10 * A.length 范围内的有效答案都将被判断为正确。

**示例 1:**
```java
输入：[3,2,4,1]
输出：[4,2,4,3]
解释：
我们执行 4 次煎饼翻转，k 值分别为 4，2，4，和 3。
初始状态 A = [3, 2, 4, 1]
第一次翻转后 (k=4): A = [1, 4, 2, 3]
第二次翻转后 (k=2): A = [4, 1, 2, 3]
第三次翻转后 (k=4): A = [3, 2, 1, 4]
第四次翻转后 (k=3): A = [1, 2, 3, 4]，此时已完成排序。 
```

**示例 2:**
```java
输入：[1,2,3]
输出：[]
解释：
输入已经排序，因此不需要翻转任何内容。
请注意，其他可能的答案，如[3，3]，也将被接受。
```

# 思路

煎饼反转就是以数组中心索引位置为轴，两两数字交换。例如最后一个数字与第一个数字交换，倒数第二个与第二个交换。

要想实现排序效果，就是依次把最大的放入最后面，这需要以下几步：

1. 找到未排序的数组里面最大的数，并记录索引。

2. 以该索引为数组边界，进行一次煎饼反转，将该数转到第一个数字。

3. 以未排序的子数组边界索引为边界，进行一次煎饼反转，将该数转到最后面。

4. 循环进行。

# 代码

```java
class Solution {
    public List<Integer> pancakeSort(int[] A) {
        List<Integer> result = new ArrayList<>();
        int size = A.length-1;
        while(size>0){
            if(findIndex(A, size) < size){
                // 两次反转将未排序子数组中最大的数字移到后面
                // result记录反转的索引位置
                result.add(findIndex(A, size)+1);
                reverse(A, 0, findIndex(A, size));
                result.add(size+1);
                reverse(A, 0, size);
            }
            size--;
        }
        
        return result;
    }
    // 煎饼反转算法
    private static void reverse(int[] A, int i, int j){
        for(;i<j;i++,j--){
            int temp = A[i];
            A[i] = A[j];
            A[j] = temp;
        }
    }
    // 寻找当前数组的最大值所在的索引位置
    private static int findIndex(int[] A, int size){
        int max=0, k=0;
        for(int i=0; i<=size; i++){
            if(A[i]>max){
                max = A[i];
                k = i;
            }
        }
        return k;
    }
}
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/pancake-sorting
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
