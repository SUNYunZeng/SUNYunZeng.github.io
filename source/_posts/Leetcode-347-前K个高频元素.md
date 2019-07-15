---
title: Leetcode 347.前K个高频元素
comments: true
toc: true
date: 2019-07-15 11:52:16
categories:
tags:
---

# 题目

给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

**示例1:**
```java
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
```

**示例2:**
```java
输入: nums = [1], k = 1
输出: [1]
```

**说明:**
```java
你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
你的算法的时间复杂度必须优于 O(n log n) , n 是数组的大小。
```

# 思路

首先用一个HashMap统计不同数字出现的次数。

然后用一个最小堆维护k大小的结果。

这里采用java的优先队列 **PriorityQueue** 去维护最小堆。

这里需要注意的一点是比较器的设计，部分源代码如下：

```java

public boolean add(E e) {
        return offer(e);
    }

public boolean offer(E e) {
        if (e == null)
            throw new NullPointerException();
        modCount++;
        int i = size;
        if (i >= queue.length)
            grow(i + 1);
        size = i + 1;
        if (i == 0)
            queue[0] = e;
        else
            siftUp(i, e);
        return true;
    }

private void siftUp(int k, E x) {
        if (comparator != null)
            siftUpUsingComparator(k, x);
        else
            siftUpComparable(k, x);
    }

private void siftUp(int k, E x) {
        if (comparator != null)
            siftUpUsingComparator(k, x);
        else
            siftUpComparable(k, x);
    }

@SuppressWarnings("unchecked")
private void siftUpComparable(int k, E x) {
    Comparable<? super E> key = (Comparable<? super E>) x;
    while (k > 0) {
        int parent = (k - 1) >>> 1;
        Object e = queue[parent];
        if (key.compareTo((E) e) >= 0)
            break;
        queue[k] = e;
        k = parent;
    }
    queue[k] = key;
}

@SuppressWarnings("unchecked")
private void siftUpUsingComparator(int k, E x) {
    while (k > 0) {
        int parent = (k - 1) >>> 1;
        Object e = queue[parent];
        // 注意：这里的比较原则是当前的与栈顶元素比较，大的就进行替换，所以维护的是最小堆栈。
        if (comparator.compare(x, (E) e) >= 0)
            break;
        queue[k] = e;
        k = parent;
    }
    queue[k] = x;
}

```

PriorityQueue方法参考[博文](https://www.cnblogs.com/Elliott-Su-Faith-change-our-life/p/7472265.html)

# 代码

```java
class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>(new Comparator<Integer>(){
            public int compare(Integer a, Integer b){
                return map.get(a) - map.get(b);
            }
        });
        int len = nums.length;
        List<Integer> result = new ArrayList<Integer>();
        // 统计各数字出现次数
        for(int i=0; i<len; ++i){
            if(map.containsKey(nums[i])){
                map.put(nums[i], map.get(nums[i])+1);
            }else{
                map.put(nums[i], 1);
            }
        }
        // 使用最小堆更新结果
        for(int i:map.keySet()){
            if(pq.size()<k){
                pq.add(i);
            }else{
                if(map.get(i)>map.get(pq.peek())){
                    pq.remove();
                    pq.add(i);
                }
            }
        }
        while(!pq.isEmpty()){
            result.add(pr.poll());
        }
        return result;
    }
}l
```