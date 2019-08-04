---
title: Leetcode 1046.最后一块石头的重量
comments: true
toc: true
date: 2019-07-30 14:36:49
categories: Algorithm
tags: Greedy Algorithm
---

# 题目

有一堆石头，每块石头的重量都是正整数。

每一回合，从中选出两块最重的石头，然后将它们一起粉碎。假设石头的重量分别为 x 和 y，且 x <= y。那么粉碎的可能结果如下：

如果 x == y，那么两块石头都会被完全粉碎；
如果 x != y，那么重量为 x 的石头将会完全粉碎，而重量为 y 的石头新重量为 y-x。
最后，最多只会剩下一块石头。返回此石头的重量。如果没有石头剩下，就返回 0。

**提示**
```java
1 <= stones.length <= 30
1 <= stones[i] <= 1000
```

# 思路

（排序 -> 选最大及第二大做差 -> 更新数组 -> 排序）（循环 length-1 次） ->最大的为结果

# 代码

```java
class Solution {
    public int lastStoneWeight(int[] stones) {
        int len = stones.length;
        for(int i=len-1; i>=1; i--){
            Arrays.sort(stones);
            stones[len-1] = stones[len-1]-stones[len-2];
            stones[len-2] = 0;
        }
        return stones[len-1];
    }
}
```

或者采用**优先堆栈**的方法维护数据的先后顺序

```java
class Solution {
    public int lastStoneWeight(int[] stones) {
        int len = stones.length;
        Queue<Integer> queue = new PriorityQueue<Integer>(new Comparator<Integer>(){
            public int compare(Integer a, Integer b){
                return (b - a);
            }
        });
        for(int i=0; i<len; i++){
            queue.add(stones[i]);
        }
        while(queue.size()>1){
            int a = queue.poll();
            int b = queue.poll();
            if(a-b != 0){
                queue.add(a-b);
            }
        }
        return queue.isEmpty()?0:queue.poll();
    }
}
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/last-stone-weight
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
