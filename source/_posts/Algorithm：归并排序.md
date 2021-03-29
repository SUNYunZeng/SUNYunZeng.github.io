---
title: 归并排序 | 插入排序 | 希尔排序
comments: true
toc: true
date: 2019-11-01 16:32:51
categories: 算法题
tags: 排序算法
---

# 插入排序
每次遍历都将对应位置的数字插入到合适的位置，当前位置之前的数据保持排序。
{% asset_img sort1.gif %}

## 代码

```Java
public class InsertionSort {
    public static int[] sort(int[] arr){
        int len = arr.length;
        int[] A = new int[len];
        System.arraycopy(arr, 0, A, 0, len);
        int i,j;
        for (i = 1; i < len; i++) {
            int temp = A[i];
            for (j = i; j > 0 && A[j - 1] > temp; j--) {
                A[j] = A[j-1];
            }
            A[j] = temp;
        }
        return A;
    }
}
```

# 希尔排序

是插入排序的变种，通过根据步长对原数组切分，加大了视野范围。

步长一直到1，此时交换的数量少，运行时间少。

```java
public class ShellSort{
    public static int[] sort(int[] nums){
        if(nums==null || nums.length==0)
            return null;
        int len = nums.length;
        int[] A = new int[len];
        System.arraycopy(nums,0,A,0,len);
        for(int step = len/2; step > 0; step/=2){
            for(int i=step; i<len; i++){
                int tmp = A[i];
                int j;
                for(j=i; j>=step && A[j-step] > tmp; j-=step)
                    A[j] = A[j-step];
                A[j] = tmp;
            }
        }
        return A;   
    }
}
```

# 归并排序

分而治之的思想。

<center>{% asset_img mergeSort.jpg %}</center>


```java
public class MergeSort {
    public static int[] sort(int[] nums){
        if(nums==null || nums.length==0)
            return null;
        int len = nums.length;
        int[] A = new int[len];
        System.arraycopy(nums, 0, A, 0, len);
        mergeSort(A, 0, A.length-1);
        return A;
    }
    private static void mergeSort(int[] nums, int i, int j){
        if(i>=j)
            return;
        int m = i + (j-i)/2;
        mergeSort(nums,i, m);
        mergeSort(nums, m+1, j);
        // m位置属于左数组
        merge(nums, i, m, j);
    }

    private static void merge(int[] nums, int i, int m, int j){
        int[] leftArr = new int[m-i+1];
        int[] rightArr = new int[j-m];
        // 辅助数组填充
        for(int l=i; l<=m; l++){
            leftArr[l-i] = nums[l];
        }
        for(int r=m+1; r<=j; r++){
            rightArr[r-m-1] = nums[r];
        }

        int l = 0, r = 0, k=i;

        while(l<leftArr.length && r<rightArr.length){
            if(leftArr[l]<rightArr[r]){
                nums[k++]=leftArr[l++];
            }else{
                nums[k++] = rightArr[r++];
            }
        }

        while(l<leftArr.length)
            nums[k++] = leftArr[l++];
        while (r<rightArr.length)
            nums[k++] = rightArr[r++];
    }
}
```
