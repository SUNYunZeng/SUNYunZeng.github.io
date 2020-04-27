---
title: 用JS写算法
comments: true
toc: true
date: 2020-04-24 18:13:50
categories: Web前端
tags: JavaScript
---

# 用JS去除字符串前后空格

```javascript
function trim(str){
    if(!str) return "";
    return trimRight(trimLeft(str));
}

function trimLeft(str){
    // 判断字符串（去掉换行、tab等）
    const judgeStr = new String(" \t\r\n");
    if(judgeStr.indexOf(str.charAt(0))!==-1){
        let j = 1, len = str.length;
        while(j<len && judgeStr.indexOf(str.charAt(j))!==-1){
            j++;
        }
        str = str.substring(j);
    }
    return str;
}

function trimRight(str){
    const judgeStr = new String(" \t\r\n");
    let i = str.length-1;
    if(judgeStr.indexOf(str.charAt(i))!==-1){
        while(i>=0 && judgeStr.indexOf(str.charAt(i))!==-1){
            i--;
        }
        str = str.substring(0, i+1);
    }
    return str;
}
```

# JS 面对经典的排序问题

## 快速排序

选择一个判断标准，然后根据这个标准将数组分割成大小两部分，然后对这两部分分别分割，直到排序完毕。

```javascript
function quickSort(arr){
    partition(arr, 0, arr.length-1);
    return arr;
}

const partition = function(arr, left, right){
    let idx = subSort(arr, left, right);
    if(left < idx-1){
        partition(arr, left, idx-1);
    }
    if(right > idx){
        partition(arr, idx, right);
    }
}

const subSort = function(arr, left, right){
    const target = arr[Math.floor((left+right)/2)];
    let i=left, j=right;
    while(i<=j){
        while(i<arr.length && arr[i]<target){
            i++;
        }
        while(j>=0 && arr[j]>target){
            j--;
        }
        if(i<=j){
            [arr[i], arr[j]] = [arr[j], arr[i]];
            i++;
            j--;
        }
    }
    return i;
}
```

+ 时间负责度

    + 如果每次选择的 target 都是中间大小的，则最优 O(nlogn)
    + 如果每次选择都是最大的（对于升序排序），则退化为冒泡排序。

+ 空间复杂度 O(1)

[参考](https://blog.csdn.net/yuzhihui_no1/article/details/44198701#t2)

## 归并排序

分治思想，先划分数组，然后依次排序，归并。

```javascript
function mergeSort(arr){
    if(arr.length<=1){
        return arr;
    }
    let idx = Math.floor(arr.length/2);
    return merge(mergeSort(arr.slice(0,idx)), mergeSort(arr.slice(idx)));
}

const merge = function(left, right){
    let i = 0, j = 0, res = [];
    while(i<left.length && j < right.length){
        if(left[i]<right[j]){
            res.push(left[i++]);
        }else{
            res.push(right[j++])
        }
    }
    while(i<left.length){
        res.push(left[i++]);
    }
    while(j<right.length){
        res.push(right[j++]);
    }
    return res;
}
```

+ 时间复杂度 O(nlogn)

+ 空间复杂度 O(n + logn) 也就是 O(n)

归并排序比较稳定，但是比较吃内存空间。快速排序不稳定，但是不消耗内存空间。


## 插入排序

```javascript
function insertSort(arr){
    for(let i=1; i<arr.length; i++){
        let temp = arr[i], j=i;
        while(j-1>=0 && arr[j-1]>temp){
            arr[j] = arr[j-1];
            j--;
        }
        arr[j] = temp;
    }
    return arr;
}
```

# 大数相加

js 里面的 Number 用64位浮点数表示，数值范围为 -2^53  ------  2^53   (包含边界)。如果数值相加后的结果超过这个范围，则会导致精度丢失。

大数相加原理是利用数组可以自动扩容来保存计算结果，并用字符串的形式输入与输出。

```javascript
function addBigNum(num1, num2){
    [num1, num2] = [num1 + '', num2 + ''];
    let len1 = num1.length, len2 = num2.length;
    for(let i=0; i<(Math.max(len1, len2)-Math.min(len1, len2)); i++){
        if(len1<len2){
            num1 = '0' + num1;
        }else{
            num2 = '0' + num2;
        }
    }
    let res = [];
    num1 = num1.split('').reverse();
    num2 = num2.split('').reverse();
    for(let i=0; i<num1.length; i++){
        res[i] = res[i]===1?res[i]:0;
        let s = res[i] + parseInt(num1[i]) + parseInt(num2[i]);
        if(s<10){
            res[i] = s;
        }else{
            res[i] = s-10;
            res[i+1] = 1;
        }
    }
    return res.reverse().join('');
}
```

# 大数相乘

思路与大数相加类似。

```javascript
function multiplyBigNUm(num1, num2){
    [num1, num2] = [num1 + '', num2 + ''];
    let [len1, len2] = [num1.length, num2.length];
    let shortNum, longNum, shortLen, longLen
    if(len1 < len2){
        [shortNum, longNum, shortLen, longLen] = [num1, num2, len1, len2];
    }else{
        [shortNum, longNum, shortLen, longLen] = [num2, num1, len2, len1];
    }
    shortNum = shortNum.split('').reverse();
    let res = '';
    for(let i=0; i<shortLen; i++){
        let temp = multiply(longNum, shortNum[i]);
        for(let j=0; j<i; j++) temp += '0';
        res = addBigNum(temp, res);
    }
    return res;
}

function multiply(num, n){
    num = num.split('').reverse();
    n = parseInt(n);
    let res = [];
    for(let i=0; i<num.length; i++){
        res[i] = res[i]?res[i]:0;
        let tmp = res[i] + parseInt(num[i])*n;
        if(tmp < 10){
            res[i] = tmp;
        }else{
            res[i] = tmp%10;
            res[i+1] = Math.floor(tmp/10);
        }
    }
    return res.reverse().join('');
}
```

# 实现 add(1,2,3) == 6, add(1,2,3)() = 6,  add(1)(2)(3) = 6, add(1)(2)(3)() = 6

```javascript
function add(){
    let res = [...arguments].reduce((a, b)=>{
        return a+b;
    }, 0);
    let innerAdd = function(){
        if(arguments.length===0){
            return res;
        }else{
            res += [...arguments].reduce((a, b)=>{
                return a+b;
            }, 0);
            return innerAdd;
        }
    }

    innerAdd.toString = function(){
        return res;
    }
    innerAdd.toValue = function(){
        return res;
    }
    return innerAdd;
}
```

# 实现 add(1)(2)(3)().res = 6, add(1,2)(3)().res = 6

```javascript
function add(){
    let res = [...arguments].reduce((a, b)=>{
        return a+b;
    }, 0);
    let obj = {res: res};
    let innerAdd = function(){
        if(arguments.length===0){
            return obj;
        }else{
            res += [...arguments].slice(1).reduce((a, b)=>{
                return a+b;
            }, 0);
            obj.res = res;
            return innerAdd;
        }
    }

    innerAdd.toString = function(){
        return res;
    }
    innerAdd.toValue = function(){
        return res;
    }
    return innerAdd;
}
```

# 实现superBaby

一个superBaby

+ 输入 
superBaby("syz")

+ 输出：
 "I am syz"

+ 输入 
superBaby("syz").eat("banana") 

+ 输出
"I am syz"
"Eating banana"

+ 输入
superBaby("syz").sleep(5000).eat("banana")

+ 输出
"I am syz"
// 等待 5000ms 后输出
"Eating banana"

+ 输入
superBaby("syz").eat("banana").sleep(5000).eat("apple").firstSleep(5000)

+ 输出
// 等待 5000ms 后输出
"I am syz"
"Eating banana"
// 等待 5000ms 后输出
"Eating apple"

```javascript
function SuperBaby(name) {
    return new _superBaby(name);
}


function _superBaby(name) {
    let that = this;
    let fn = function () {
        console.log(`I am ${name}`);
        that.next();
    }
    this.task.push(fn);
    setTimeout(()=>this.next(), 0);
}

_superBaby.prototype = {
    constructor: _superBaby,
    task: [],
    next: function () {
        let fn = this.task.shift();
        fn && fn();
    },
    eat: function (food) {
        let that = this;
        let fn = function () {
            console.log(`Eating ${food}`);
            that.next();
        }
        this.task.push(fn);
        return this;
    },
    sleep: function(time){
        let that = this;
        let fn = function(){
            setTimeout(()=>that.next(), time);
        };
        this.task.push(fn);
        return this;
    },
    sleepFirst: function(time){
        let that = this;
        let fn = function(){
            setTimeout(()=>that.next(), time);
        };
        this.task.unshift(fn);
        return this;
    }
}

SuperBaby("syz").sleep(5000).eat("banana").sleepFirst(5000);
```

# 手写 apply、call 及 bind

```javascript
// apply
Function.myApply = function(context, args){
    if( typeof this !== 'function'){
        throw new TypeError('Error');
    }
    context = context || window;
    args = args || [];
    context.fn = this;
    const res = context.fn(...args);
    delete context.fn;
    return res;
}

// call
Function.myCall = function(context, ...args){
    if(typeof this !== 'function'){
        throw new TypeError('Error');
    }
    context = context || window;
    context.fn = this;
    const res = context.fn(...args);
    delete context.fn;
    return res;
}

// bind
Function.muBind = function(context){
    if (typeof this !== 'function') {
        throw new TypeError('Error')
    }
    let that = this;
    let args = [...arguments].slice(1);
    context = context || window;
    return function(){
        that.apply(context, args.concat([...arguments]))
    }
}
```