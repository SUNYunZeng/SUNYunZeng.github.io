---
title: Leetcode 155.最小栈
comments: true
toc: true
date: 2019-06-22 14:07:29
categories: Algorithm
tags: Other Algorithm
---

# 题目

设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) -- 将元素 x 推入栈中。
pop() -- 删除栈顶的元素。
top() -- 获取栈顶元素。
getMin() -- 检索栈中的最小元素。

**示例:**
```java
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

# 思路1

可以用两个栈，一个栈用来维护当前栈内最小的元素，一个栈用来维度当前栈内的元素。

属于投机取巧的方法。

# 代码
```java
class MinStack {
    
    private Stack<Integer> stack;
    private Stack<Integer> minStack;

    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack<>();
        minStack = new Stack<>();
    }
    
    public void push(int x) {
        stack.push(x);
        if(minStack.isEmpty() || x <= minStack.peek()){
            minStack.push(x);
        }
    }
    
    public void pop() {
        // 注意，peek()方法返回的是Integer对象，而java在比较Integer时数据范围在-128~127用 == 比较是相等的，但是
        // 超过这个范围就是不相等的。这是因为常量池范围内地Integer对象都是同一个，而 == 是比较对象是否为同一个对象
        // 如果比较数值是否相等，应该采用Integer重写的equals()方法!!!
        if(stack.peek().equals(minStack.peek())){
            minStack.pop();
        }
        stack.pop();
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        return minStack.peek();  
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

# 思路2

自己用数组实现元素栈与存储最小元素的栈。

要点，数组的自动扩容。

```java
class MinStack {
    
    private int[] minValue;
    private int[] data;
    private int index;

    /** initialize your data structure here. */
    public MinStack() {
        minValue = new int[10];
        data = new int[10];
        index = -1;
    }
    
    public void push(int x) {
        // 扩容
        if(++index>data.length-1)
            resize();
        data[index] = x;
        if(index==0 || x <minValue[index-1]){
            minValue[index]=x;
        }else{
            minValue[index] = minValue[index-1];
        }
    }
    
    public void pop() {
        index--;
    }
    
    public int top() {
        return data[index];
    }
    
    public int getMin() {
        return minValue[index];
    }
    public void resize(){
        int[] newdata = new int[data.length*2];
        int[] newMinValue = new int[data.length*2];
        System.arraycopy(data,0,newdata,0,data.length);
        System.arraycopy(minValue,0,newMinValue,0,minValue.length);
        data = newdata;
        minValue = newMinValue;
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

或者
```java
class MinStack {
    
    private int minValue;
    private int[] data;
    private int index;

    /** initialize your data structure here. */
    public MinStack() {
        minValue = Integer.MAX_VALUE;
        data = new int[10];
        index = -1;
    }
    
    public void push(int x) {
        // 扩容
        if(++index>data.length-1)
            data = Arrays.copyOf(data, data.length*2);
        data[index] = x;
        if(x < minValue){
            minValue = x;
        }
    }
    
    public void pop() {
        index--;
        if(data[index+1]==minValue){
            minValue = Integer.MAX_VALUE;
            for(int i=0; i<=index; i++){
                if(data[i]<minValue){
                     minValue = data[i];
                }     
            }
        }
    }
        
    public int top() {
        return data[index];
    }
    
    public int getMin() {
        return minValue;
    }   
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/min-stack
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。