---
title: Algorithm-排序算法
toc: true
date: 2019-05-27 21:38:08
categories: Algorithm
tags: Sort
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

# 冒泡排序

每次将最大的(或最小的)沉入数组最底层。
{% asset_img bubble.gif %}
## 代码

```java
public class BubbleSort {
    public static int[] sort(int[] arr) {
        int len = arr.length;
        int[] A = new int[len];
        System.arraycopy(arr,0, A, 0, len);
        for (int i = 1; i < len; i++) {
            for (int j = 0; j < len-i; j++) {
                if(A[j]>A[j+1]){
                    // 无需中介的数值互换方法
                    A[j] += A[j+1];
                    A[j+1] = A[j] - A[j+1];
                    A[j] -= A[j+1];
                }
            }
        }
        return A;
    }
}
```

转自[知乎](https://zhuanlan.zhihu.com/p/52884590)