---
title: 剑指Offer：最小的K个数
comments: true
toc: true
date: 2019-10-13 15:52:01
categories: 算法题
tags: 剑指Offer
---

# 题目

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

# 思路1 + 代码

维护一个最小堆，该最小堆即为所求。

类似<u>[题目](http://sunyunzeng.cn/Leetcode-347-%E5%89%8DK%E4%B8%AA%E9%AB%98%E9%A2%91%E5%85%83%E7%B4%A0/)</u>

```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        if(input==null || input.length==0 || k>input.length)
            return new ArrayList<Integer>();
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>((o1, o2) -> o2-o1);
        for(int num:input){
            pq.add(num);
            if(pq.size()>k)
                pq.poll();
        }
        return new ArrayList<Integer>(pq);
    }
}
```

# 思路2 + 代码

该题完全可以先排序后取最小的K个数。

因此排序方法的选取很重要。

**## 冒泡方法**

只用排k个最小值即可，因此最外圈循环K次。

```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        if(input==null || input.length==0 || k>input.length)
            return new ArrayList<Integer>();
        int len = input.length;
        ArrayList<Integer> res = new ArrayList<Integer>();
        for(int i=1; i<k+1; i++){
            for(int j=0; j<len-i; j++){
                if(input[j]<=input[j+1]){
                    int tmp = input[j];
                    input[j] = input[j+1];
                    input[j+1] = tmp;
                }
            }
            res.add(input[len-i]);
        }
        return res;
    }
}
```

**## 快速排序**

<font color=#f07c82>[快排](http://sunyunzeng.cn/Algorithm-%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95/)</font>关键在于<font color=#f07c82>基准</font>的选择，大于基准的数放于一边，小于基准的数放在另一边。

我们的标准是划分的位置刚好是在第**k**个位置，这样 k 位置之前的数组都为排序好的。

```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        if(input==null || input.length==0 || k>input.length)
            return new ArrayList<Integer>();
        int len = input.length;
        ArrayList<Integer> res = new ArrayList<Integer>();
        sort(input, 0, input.length-1, k-1);
        for(int i=0; i<k; i++){
            res.add(input[i]);
        }
        return res;
    }
    private void sort(int[] input, int low, int high, int k){
        int l = low;
        int h = high;
        while(l<h){
            int tmp = partition(input, l, h);
            if(tmp==k)
                break;
            if(tmp>k){
                h = tmp-1;
            }else{
                l = tmp+1;
            }
        }
    }
    
    private int partition(int[] input, int l, int h){
        int criterion = input[h];
        int i = l-1;
        int j = h;
        while(true){
            while(i<j && input[++i]<criterion);
            while(i<j && input[--j]>criterion);
            if(i>=j)
                break;
            swap(input, i, j);
        }
        swap(input,i, h);
        return i;
    }
    private void swap(int[] arr, int a, int b){
        int tmp = arr[a];
        arr[a] = arr[b];
        arr[b] = tmp;
    }
}
```

