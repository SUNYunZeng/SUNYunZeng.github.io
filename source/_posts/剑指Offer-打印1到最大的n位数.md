---
title: 剑指Offer 打印1到最大的n位数
comments: true
toc: true
date: 2019-09-24 21:23:35
categories: Algorithm
tags: 剑指Offer
---

# 题目 

打印1到最大的n位数。

**示例：**
```java
输入 n = 1

打印 1 2 3 4 5 6 7 8 9
```

# 思路 + 代码

常规操作是<font color=#f07c82>暴力循环打印</font> 1 到 n 位数，但是大数问题，存在数值溢出。

采用<font color=#f07c82>字符串</font>或者<font color=#f07c82>数组</font>存储中间结果。

## 思路1

用一个 长度为 n 的数组存储，然后分别在最后一位计算，<font color=#f07c82>每次加一</font>，逢十进一。

```java
public class PrintToNNumber {
    private static boolean increment(int[] nums) {
        if(nums==null || nums.length==0)
            return false;
        // 相加是否大于10
        int overflow = 0;
        int len = nums.length;
        for (int i = len - 1; i >= 0; i--) {
            int nSum = overflow + nums[i];

            // 只在最后一位加
            if(i==len-1){
                nSum++;
            }
            
            if (nSum < 10) {
                nums[i] = nSum;
                return true;
            }

            // >= 10
            // 超出范围
            if(i==0){
                return false;
            }
            nums[i] = 0;
            overflow = 1;
        }
        return false;
    }

    // 清楚掉数组前面的零
    public static void printArray(int[] nums) {
        int len = nums.length;
        boolean isZero = true;
        for (int i = 0; i < len; i++) {
            if(nums[i]!=0)
                isZero = false;
            if (!isZero) {
                System.out.print(nums[i]);
            }
        }
        System.out.println();
    }

    public static void main(String[] args) {
        int n = 3;
        int[] nums = new int[n];
        while (increment(nums)){
            printArray(nums);
        }
    }
}

```

## 思路2

使用<font color=#f07c82>递归方法</font>，递归到组后一位，然后依次往前打印。

```java
private static void recursive(int[] nums, int index) {
        if (nums.length == index) {
            printArray(nums);
            return;
        }
        for (int i = 0; i < 10; i++) {
            nums[index] = i;
            recursive(nums, index+1);
        }
    }

public static void main(String[] args) {
        int n = 3;
        int[] nums = new int[n];
        recursive(nums, 0);
    }
```