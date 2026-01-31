---
title: 自己动手实现符合Promise/A+规范的Promise
comments: true
toc: true
date: 2020-04-14 12:06:19
categories: Web前端
tags: JavaScript
---

# Promise异步实现

之前在[JS异步函数小结](http://sunyunzeng.cn/JS%E5%BC%82%E6%AD%A5%E5%87%BD%E6%95%B0%E5%B0%8F%E7%BB%93/)里面初步介绍了JS里面的一些异步操作与 Promise 的使用方法，我们要知其然也要知其所以然，所以让我们看一下如何动手实现自己的 MyPromise。

## Promise/A+协议

协议是实现的基础，Promise/A+协议的中文参考网站如下：

https://www.ituring.com.cn/article/66566

英文网站如下：

https://promisesaplus.com/

# 实现最简单的构造函数，可以满足回调函数的调用

+ 首先Promise**构造参数只能接受函数**，否则报错。

+ Promise**内部变量**包括：
    - 值 **value** => 指任何 JavaScript 的合法值（包括 undefined , thenable 和 promise）。
    - 拒因 **reason** => 值一个 promise 拒绝的原因。
    - 状态 **state** => promise 执行所处的状态
        + Promise规范规定了Promise的状态一共有三种：**Pending、Fulfilled 及 Rejected**。状态之间的转换只能是 Pending => Fulfilled 或者 Pending => Rejected，状态的转换不可逆转。

初步实现：

```javascript
// MyPromise.js

class MyPromise{
    constructor(executor){
        // Promise的构造函数参数必须是函数，否则报错
        if(typeof executor !== "function"){
            throw new TypeError(`Promise resolver ${executor} is not a function`);
        }

        this.initValue();
    }

    // 内部参数初始化
    initValue(){
        this.value = null; // 值
        this.reason = null; //据因
        this.state = MyPromise.PENDING; // 初始状态
    }
}

MyPromise.PENDING = "Pending";
MyPromise.FULFILLED = "Fulfilled";
MyPromise.REJECTED = "Rejected";
```

+ 同时 Promise **采用两个回调函数作为值 value 传递**的方法，分别是 **resolve** 函数 及 **reject** 函数，分别对应 **Fulfilled** 状态及 **Rejected** 状态的函数回调，回调函数分别为 **OnFulfilled** 及 **OnRejected**。

```javascript
class MyPromise{
    // ...

    then(OnFulfilled, OnRejected){
        // promise 执行成功后回调
        if(this.state === MyPromise.FULFILLED){
            OnFulfilled(this.value);
        }

        // promise 执行失败后回调
        if(this.state === MyPromise.REJECTED){
            OnRejected(this.value);
        }
    }
}
```

**测试**

```javascript

let promise = new MyPromise('1'); // 报错 TypeError: Promise resolver 1 is not a function

let promise2 = new MyPromise((resolve, reject)=>{
    resolve(1);
});

promise2.then(data=>console.log(data)); // 打印 1可以执行
```

**问题**

如果promise函数内存在异步，那么 state 状态仍为 Pending 但是 then 函数为同步执行，导致 value 没有同步传递，没有输出。

```javascript
let promise3 = new MyPromise((resolve, reject)=>{
    setTimeout(()=>{
        resolve(1);
    }) 
});

promise3.then(data=>console.log(data)) // 没有输出
```

**解决**

添加 state 为 Pending 状态时的处理，并用两个任务队列 **OnFulfilledCallbacks** 及 **OnRejectedCallbacks** 分别存储状态为 **OnFulfilled** 及 **OnRejected** 的异步执行回调函数，然后等 resolve 函数执行完毕再调用回调函数。

```javascript
// MyPromise.js

// resolve 函数执行，state 变为 FULFILLED
resolve(value) {
    // ...
    this.OnFulfilledCallbacks.forEach(cb => cb(this.value));
}

// reject 函数执行，state 变为 REJECTED
reject(reason) {
    // ...
    this.OnRejectedCallbacks.forEach(cb => cb(this.reason));
}

then(OnFulfilled, OnRejected) {
        // ...

        // 异步执行函数状态为 Pending 时的处理
        if (this.state === MyPromise.PENDING) {

            this.OnFulfilledCallbacks.push(
                (value) => setTimeout(() => {
                    OnFulfilled(value);
                }));

            this.OnRejectedCallbacks.push(
                (reason) => setTimeout(() => {
                    OnRejected(reason);
                }));
        }
    }
```

```javascript
let promise3 = new MyPromise((resolve, reject)=>{
    setTimeout(()=>{
        resolve(1);
    }) 
});

promise3.then(data=>console.log(data)) // 输出 1
```

# 链式调用

根据 Promise/A+ 的规范定义，Promise的 **then 函数返回一个 promise2 以支持链式调用**，因此对代码进行修改：

```javascript
then(OnFulfilled, OnRejected) {
    if(typeof OnFulfilled !== "function"){
        // 如果 OnFulfilled 不是函数或者为空就支持链式调用 .then().then()
        OnFulfilled = function(value){
            return value;
        }
    }

    if(typeof OnRejected !== "function"){
        OnRejected = function(reason){
            throw reason;
        }
    }

    let promise2 = new MyPromise((resolve, reject) => {
        // promise 执行成功后回调
        if (this.state === MyPromise.FULFILLED) {
            setTimeout(() => {
                // 上一个 Promise回调函数返回的值交给下一个 Promise调用
                try{
                    const x = OnFulfilled(this.value);
                    resolve(x); 
                }catch(e){
                    reject(e);
                }
            })
        }

        // promise 执行失败后回调
        if (this.state === MyPromise.REJECTED) {
            setTimeout(() => {
                try{
                    const x = OnRejected(this.reason);
                    resolve(x);
                }catch(e){
                    reject(e);
                }
            })
        }

        // 异步执行函数状态为 Pending 时的处理
        if (this.state === MyPromise.PENDING) {

            this.OnFulfilledCallbacks.push(
                (value) => setTimeout(() => {
                    try{
                        const x = OnFulfilled(value);
                        resolve(x);
                    }catch(e){
                        reject(e);
                    }
                }));

            this.OnRejectedCallbacks.push(
                (reason) => setTimeout(() => {
                    try{
                        const x = OnRejected(reason);
                        resolve(x);
                    }catch(e){
                        reject(e);
                    }
                }));
        }
    });
    return promise2;
}
```

**测试**

```javascript
let promise4 = new MyPromise((resolve, reject)=>{
    setTimeout(()=>{
        resolve(2);
    }) 
});

promise4.then().then().then(data=>console.log(data)) // 2 支持链式调用
```

# 如果 resolve 函数返回 Promise 或者 带有 thenable 方法的类及 then 方法的处理

首先定义一个 resolveMyPromise 的处理方法

```javascript
MyPromise.resolveMyPromise = function(promise2, x, resolve, reject){
    // 循环调用 thenable 方法时只调用一次
    let called = false;

    // 如果返回的 x 等于 promise2，则报错，避免循环调用
    if(x===promise2){
        return reject(new new TypeError("cannot return the same promise object from onfulfilled or on rejected callback."))
    }

    if(x instanceof MyPromise){

    }
    // 避免null也为object
    else if( (x!==null && typeof x === "object") || typeof x ==="function"){

    }else{
        // 如果不是 MyPromise 或者不是具有 thenable 的类及方法，就直接传入 x 数据
        resolve(x);
    }
}
```

根据 Promise/A+ 的规范依次实现 resolveMyPromise 方法

```javascript


MyPromise.resolveMyPromise = function(promise2, x, resolve, reject){
    // 循环调用 thenable 方法时只调用一次
    let called = false;

    // 如果 promise2 和 x 指向同一对象，以 TypeError 为据因拒绝执行 promise
    if(x===promise2){
        reject(new new TypeError("cannot return the same promise object from onfulfilled or on rejected callback."))
    }

    // x 为 Promise
    if(x instanceof MyPromise){
        // 如果 x 为 Promise ，则使 promise 接受 x 的状态
        // 如果 x 处于执行态，用相同的值执行 promise
        // 如果 x 处于拒绝态，用相同的据因拒绝 promise
        x.then(y=>{
            MyPromise.resolveMyPromise(promise2, y, resolve, reject);
        }, r=>{
            reject(r);
        })
    }
    // 如果 x 为对象或者函数 (x !== null 避免null也为object)
    else if( (x!==null && typeof x === "object") || typeof x ==="function"){
        try{ // 防止调用 then 的 getter 方法调用抛出异常
            // 把 x.then 赋值给 then
            const then = x.then;
            // 如果 then 是函数，将 x 作为函数的作用域 this 调用之。
            // 传递两个回调函数作为参数，第一个参数叫做 resolvePromise ，第二个参数叫做 rejectPromise:
            if(typeof then === "function"){
                then.call(x, y=>{
                    // 如果 resolvePromise 和 rejectPromise 均被调用，
                    // 或者被同一参数调用了多次，则优先采用首次调用并忽略剩下的调用
                    if(called) return;
                    called = true;
                    MyPromise.resolveMyPromise(promise2, y, resolve, reject);
                }, r=>{
                    if(called) return;
                    called = true;
                    reject(r);
                })
            }else{
                if(called) return;
                called = true;
                resolve(x);
            }
            // 
        }catch(e){
            if(called) return;
            called = true;
            reject(e);
        }
    }else{
        // 如果不是 MyPromise 或者不是具有 thenable 的类及方法，就直接传入 x 数据
        resolve(x);
    }
}
```

再在 then 方法里面调用

```javascript
then(OnFulfilled, OnRejected) {
    if(typeof OnFulfilled !== "function"){
        // 如果 OnFulfilled 不是函数或者为空就支持链式调用 .then().then()
        OnFulfilled = function(value){
            return value;
        }
    }

    if(typeof OnRejected !== "function"){
        OnRejected = function(reason){
            throw reason;
        }
    }

    let promise2 = new MyPromise((resolve, reject) => {
        // promise 执行成功后回调
        if (this.state === MyPromise.FULFILLED) {
            setTimeout(() => {
                // 上一个 Promise回调函数返回的值交给下一个 Promise调用
                try{
                    const x = OnFulfilled(this.value);
                    MyPromise.resolveMyPromise(promise2, x, resolve, reject); 
                }catch(e){
                    reject(e);
                }
            })
        }

        // promise 执行失败后回调
        if (this.state === MyPromise.REJECTED) {
            setTimeout(() => {
                try{
                    const x = OnRejected(this.reason);
                    MyPromise.resolveMyPromise(promise2, x, resolve, reject);
                }catch(e){
                    reject(e);
                }
            })
        }

        // 异步执行函数状态为 Pending 时的处理
        if (this.state === MyPromise.PENDING) {

            this.OnFulfilledCallbacks.push(
                (value) => setTimeout(() => {
                    try{
                        const x = OnFulfilled(value);
                        MyPromise.resolveMyPromise(promise2, x, resolve, reject);
                    }catch(e){
                        reject(e);
                    }
                }));

            this.OnRejectedCallbacks.push(
                (reason) => setTimeout(() => {
                    try{
                        const x = OnRejected(reason);
                        MyPromise.resolveMyPromise(promise2, x, resolve, reject);
                    }catch(e){
                        reject(e);
                    }
                }));
        }
    });
    return promise2;
}
```

# 正确测试

+ 安装 promises-aplus-tests 测试脚本

```javascipt
npm install promises-aplus-tests -D
npx promises-aplus-tests promise.js
```

+ 在我们 MyPromise.js 文件夹末尾添加测试 deferred 钩子

```javascript
MyPromise.deferred = function () {
    const defer = {}
    defer.promise = new MyPromise((resolve, reject) => {
        defer.resolve = resolve
        defer.reject = reject
    })
    return defer
}

module.exports = MyPromise;
```

+ 运行代码测试

```javascript
npx promises-aplus-tests MyPromise.js
```

+ 结果

{% asset_img res.png %}