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

转自[知乎](https://zhuanlan.zhihu.com/p/52884590)(插入、冒泡图片来源，侵删)

# 快速排序

每次找出一个基准，作为划分标准。方法**类似于二分法，每次都把小于基准的数放于基准数组的左边，把大于基准的数放于数组的右边**。

算法不稳定，**时间复杂度**最好$O(nlog(n))$，最差$O(n^2)$

{% asset_img qucikSort.gif %}

当数组本来是排序好的，例如是顺序排列，此时要计算逆序排列，如果利用快排方法，则时间复杂度为$O(n^2)$，当然，此时的基准选取是头部或者尾部。

所以**基准的选取**很重要，常见得有随机选取、取首或末端的数及三数中值法（取首末及中间位置三个数的中值）。

理论上，选取的基准是**所有数的中位数**，则划分最均匀，用时最短。

## 选取末尾值作为基准

```java
public class QuickSort {
    public int[] sort(int[]arr){
        int len = arr.length;
        if(len==0) return arr;
        // 在新数组上进行排序而不影响原始数组
        int[]A = new int[len];
        System.arraycopy(arr, 0, A, 0, len);
        quickSort(A, 0, len-1);
        return A;
    }
    private void quickSort(int[]A, int left, int right){
        if(left>right) return;
        // 获取基准值
        int pivot = partition(A, left, right);
        quickSort(A, left, pivot-1);
        quickSort(A, pivot+1, right);        
    }
    private int partition(int[]A, int left, int right){
        // 以末端的数字为基准
        int criterion = A[right];
        int i = left;
        int j = right;
        while(i<j){
            // 必须左边的哨兵先走，否则交换数值会出错
            while(i<j && A[i]<=criterion){
                i++;
            }
            while(i<j && A[j]>=criterion){
                j--;
            }
            if(i<j){
                int temp = A[i];
                A[i] = A[j];
                A[j] = temp;
            }
        }
        A[right] = A[i];
        A[i] = criterion;
        return i;
    }
}
```

## 优化的快速排序

采用**三值法作为划分基准，并在数据量不大时采用插入排序，提高效率。**

```java
public class QuickSort {
    private int threshold = 4;
    QuickSort(){};

    QuickSort(int i) {
        this.threshold = i;
    }
    public int[] sort(int[]arr){
        int len = arr.length;
        if(len==0) return arr;
        // 在新数组上进行排序而不影响原始数组
        int[]A = new int[len];
        System.arraycopy(arr, 0, A, 0, len);
        quickSort(A, 0, len-1);
        return A;
    }
    private void quickSort(int[]A, int left, int right){
        if(left>right) return;
        // 小数据情况下递归方法需要调用栈，效率较低，此时采用插入排序方法
        if(right-left>4){
            // 获取基准值
            int pivot = partition(A, left, right);
            quickSort(A, left, pivot-1);
            quickSort(A, pivot+1, right);
        }else {
            insertionSort(A);
        }
    }
    private int partition(int[]A, int left, int right){
        // 以末端的数字为基准
        int criterion = getMedianPivot(A, left, right);
        // 从第二个及倒数第二个开始，因为getMedianPivot已经对首末数值进行了排序
        int i = left+1;
        int j = right-1;
        while(i<j){
            // 必须左边的哨兵先走，否则交换数值会出错
            while(i<j && A[i]<=criterion){
                i++;
            }
            while(i<j && A[j]>=criterion){
                j--;
            }
            if(i<j){
                int temp = A[i];
                A[i] = A[j];
                A[j] = temp;
            }
        }
        A[right-1] = A[i];
        A[i] = criterion;
        return i;
    }

    private int getMedianPivot(int[]A, int left, int right){
        int center = (left+right)/2;
        // 对于首末及中间的数进行排序
        if (A[left] > A[center]) {
            swap(A, left, center);
        }
        if (A[center] > A[right]) {
            swap(A, center, right);
        }
        if (A[left] < A[right]) {
            swap(A, left, right);
        }
        //交换中间与倒数第二个数，因为经过排序，最后一个已经是最大
        swap(A, center, right-1);
        return A[right-1];
    }

    private void swap(int[] A, int i, int j) {
        int temp = A[i];
        A[i] = A[j];
        A[j] = temp;
    }

    private void insertionSort(int[] A) {
        int len = A.length;
        int i,j;
        for (i = 0; i < len; i++) {
            int temp = A[i];
            for (j = i; j > 0 && A[j] < A[j - 1]; j--) {
                A[j] = A[j-1];
            }
            A[j] = temp;
        }
    }
}
```

[1.参考资料](https://zhuanlan.zhihu.com/p/52884590)      [2.参考资料](https://blog.csdn.net/shujuelin/article/details/82423852)

[十大排序算法讲解推荐](https://mp.weixin.qq.com/s/IAZnN00i65Ad3BicZy5kzQ)(快排图片来源，侵删)