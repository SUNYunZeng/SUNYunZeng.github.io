---
title: Leetcode 739.每日温度
comments: true
toc: true
date: 2019-07-12 20:46:54
categories: Algortihm
tags: Other Algorithm
---

# 题目

根据每日 气温 列表，请重新生成一个列表，对应位置的输入是你需要再等待多久温度才会升高超过该日的天数。如果之后都不会升高，请在该位置用 0 来代替。

例如，给定一个列表 temperatures = [73, 74, 75, 71, 69, 72, 76, 73]，你的输出应该是 [1, 1, 4, 2, 1, 1, 0, 0]。

提示：气温 列表长度的范围是 [1, 30000]。每个气温的值的均为华氏度，都是在 [30, 100] 范围内的整数。

# 思路+代码

最简单的思路，两次循环。

```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        int len = T.length;
        int[] results = new int[len];
        for(int i=0; i<len-1; i++){
            int tmp = 0;
            for(int j=i+1; j<len; j++){
                if(T[j]>T[i]){
                    results[i]=tmp+1;
                    break;
                }
            Q    tmp++;
            }
        }
        return results;
    }
}
```

**利用栈**

栈的特性是先进后出，所以需要从后往前遍历。

维护一个递减数据的索引（天）序列。

如果当前数据（温度）大于等于栈顶索引（天）对应的数据（温度），则更新栈内数据，保持递减特性。

否则计算当前数据索引（天）与栈顶所索引（天）为结果值。

```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        int len = T.length;
        int[] results = new int[len];
        Stack<Integer> stack  = new Stack<>();
        for(int i=len-1; i>=0; --i){
            while(!stack.isEmpty() && T[i] >= T[stack.peek()]){
                stack.pop();
            }
            if(!stack.isEmpty())
                // 更新结果值
                results[i] = stack.peek() - i;
            // 维护递减序列
            stack.push(i);             
        }
        return results;
    }
}
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/daily-temperatures
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
