---
title: LeetCode 621.任务调度器
comments: true
toc: true
date: 2019-11-02 16:16:15
categories: 算法题
tags: LeetCode
---

# 题目

给定一个用字符数组表示的 CPU 需要执行的任务列表。其中包含使用大写的 A - Z 字母表示的26 种不同种类的任务。任务可以以任意顺序执行，并且每个任务都可以在 1 个单位时间内执行完。CPU 在任何一个单位时间内都可以执行一个任务，或者在待命状态。

然而，两个相同种类的任务之间必须有长度为 n 的冷却时间，因此至少有连续 n 个单位时间内 CPU 在执行不同的任务，或者在待命状态。

你需要计算完成所有任务所需要的最短时间。

**示例1：**

```java
输入: tasks = ["A","A","A","B","B","B"], n = 2
输出: 8
执行顺序: A -> B -> (待命) -> A -> B -> (待命) -> A -> B.
``` 

**注:**

```java
1. 任务的总个数为 [1, 10000]。
2. n 的取值范围为 [0, 100]。
```

# 思路 + 代码

## 方法1

规定 n + 1 个任务为一轮，这样的好处是同一轮中一个任务最多只能被安排一次。在每一轮中，我们将当前的任务按照它们剩余的次数降序排序，并选择剩余次数最多的 n + 1 个任务依次执行。如果任务的种类 t 少于 n + 1 个，就只选择全部的 t 种任务，其余的时间空闲。这样做的正确性在于，由于冷却时间的存在，出现次数较多的那些任务如果不尽早安排，将会导致大量空闲时间的出现，因此贪心地将出现次数较多的任务安排在前面是合理的。同时我们可以保证，这一轮的第 k 个任务距离上一次执行至少有 n 个单位的冷却时间。我们可以使用逆向思维来证明：假设第 r 轮中某个任务在第 k 个执行，那么说明它在第 r 轮时为数量第 k 多的任务。在第 r 轮结束后，第 1 多到第 k 多的任务的数量都会减少 1，因此在第 r + 1 轮，这个任务最多也只能是数量第 k 多，因此它如果被执行，一定满足冷却时间的要求。

```java
public class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] map = new int[26];
        for (char c: tasks)
            map[c - 'A']++;
        Arrays.sort(map);
        int time = 0;
        while (map[25] > 0) {
            int i = 0;
            while (i <= n) {
                if (map[25] == 0)
                    break;
                if (i < 26 && map[25 - i] > 0)
                    map[25 - i]--;
                time++;
                i++;
            }
            Arrays.sort(map);
        }
        return time;
    }
}
```

## 方法2

(count[25] - 1) * (n + 1) + maxCount


假设数组 ["A","A","A","B","B","C"]，n = 2，A的频率最高，记为count = 3，所以两个A之间必须间隔2个任务，才能满足题意并且是最短时间（两个A的间隔大于2的总时间必然不是最短），因此执行顺序为： A->X->X->A->X->X->A，这里的X表示除了A以外其他字母，或者是待命，不用关心具体是什么，反正用来填充两个A的间隔的。上面执行顺序的规律是： 有count - 1个A，其中每个A需要搭配n个X，再加上最后一个A，所以总时间为 (count - 1) * (n + 1) + 1
要注意可能会出现多个频率相同且都是最高的任务，比如 ["A","A","A","B","B","B","C","C"]，所以最后会剩下一个A和一个B，因此最后要加上频率最高的不同任务的个数 maxCount
公式算出的值可能会比数组的长度小，如["A","A","B","B"]，n = 0，此时要取数组的长度

```java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        if(tasks==null || tasks.length==0 || n<0)
            return 0;
        int len = tasks.length;
        int[] counts = new int[26];
        // 统计数量
        for(char c: tasks){
            counts[c-'A']++;
        }
        int maxCount = 0;
        Arrays.sort(counts);
        for(int i=0; i<26; i++)
            if(counts[i]==counts[25])
                maxCount++;
        int res = (counts[25]-1)*(n+1)+maxCount;
        
        return res<len?len:res;
    }
}
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/task-scheduler
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
