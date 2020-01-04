---
title: JS异步函数小结
comments: true
toc: true
date: 2020-01-04 15:49:12
categories: Web前端
tags: JavaScript
---

# JS的异步

JavaScript的执行环境是**单线程的**，对于http事件触发线程、浏览器事件触发线程、浏览器定时器等浏览器会单独开辟出一个异步线程处理，处理完毕后，加入任务队列，等待JS主线程调用执行。

例如：
```javascript
setTimeout(()=>console.log('触发了'),0);
console.log('我先触发');

// 输出
/*
我先触发
触发了
*/
```

虽然setTimeout()被设置为马上触发，但是setTimeout触发的异步任务需先放在任务队列中，等主线程中console()函数执行完毕后，再能被触发。

> JavaScript执行环境(浏览器)是从头到尾一行一行往下执行，但是遇到异步任务，先放入任务队列，等待主线程可以执行该任务，才被执行。

详细的JS代码执行顺序可查看 https://juejin.im/post/59e85eebf265da430d571f89

异步程序在JS代码中很常见，因为Web应用总归要与远方的服务器交互，请求数据，这个过程需要异步进行。否则，浏览器会一直卡住，直到结果请求完毕。

我们先看一下最简单的回调函数：setTimeout() 与 setsetInterval()，它们都是Windows对象的自带方法。

## setTimeout

setTimeout()表示指定一个异步程序，在规定的时间后执行，但不保证百分百执行。

```javascript
// 2000ms后执行 console.log()
// 方法一
setTimeout(()=> console.log("执行啦！"), 2000)

// 方法二
let innerFun = function(test){
    console.log(test);
}
setTimeout(innerFun, 2000, '执行啦！');
```

如果前面有耗时的密集计算，哪怕setTimeout()设定了在很短的时间后执行，也要等主线程执行完毕，所以说 setTimeout()的延迟时间为理想时间。

```javascript
// 预期1s后执行console.log()
let s = Date.now();
let innerFun = function(){
    console.log((Date.now()-s)/1000);
}
setTimeout(innerFun, 1000);
let m = 0;
for(let i=1; i<100000;i++){
    for(let j =0; j<i; j++)
        m += j;
}
// 实际5.547后执行
```

clearTimeout() 方法可取消由 setTimeout() 方法设置的定时操作。

```javascript
let timeout = setTimeout(() => console.log("执行!"), 1000);
clearTimeout(timeout);
```

## setInterval

setInterval()跟setTimeout用法类似，只不过不是执行一次，而是如果不停止就永远重复执行。

```javascript
// 每1000ms执行一次setInterval
let i =0;
let interval = setInterval(()=>console.log(i++), 1000)
```

clearInterval()可以清除interval操作。

```javascript
clearInterval(interval);
```

setInterval()指定的时间也是理想的时间，该时间包括程序本身的运行时间。如果程序本身运行时间大于指定时间，则setInterval()无法满足间隔时间定时运行。

```javascript
let s = Date.now();
innerFun = function(){
    let m = 0;
    for(let i=1; i<100000;i++){
        for(let j =0; j<i; j++)
            m += j;
    }
    console.log((Date.now() - s)/1000)
}

setInterval(innerFun, 1000)
// 每次的运行时间都远远大于1s
```

如果你想无论程序执行多长时间，每间隔固定时间都触发程序，你可以利用setTimeout()递归实现：

```javascript
let i = 1;
let run = setTimeout(function recur(){
    console.log(i++);
    setTimeout(recur, 1000);
}, 1000);
```

上面的程序可以保证每隔1000ms就执行console.log()方法，而不用管程序的执行时间。

> setTimeout() 与 setInterval()都是系统自带的基础的异步程序添加方法，可以时间指定延迟时间触发程序。

旧版本JS的异步是依靠回调函数实现的，例如传统的AJAX：

```javascript
$.get('ajax/text.html', function(data){
    // doSomeThing
})

request.onreadystatechange = function () {
    if (request.readyState === 4) {
        if (request.status === 200) {
            return success(request.responseText);
        } else {
            return fail(request.status);
        }
    }
}
```

但是回调函数之间往往存在依赖关系，即下一次回调函数的执行依赖于上一次回调函数的执行结果，然后，当这种依赖变得越来越多，代码的可读性就变得很差，此时称为“回调地狱”。

```javascript
// callback hall
chooseToppings(function(toppings) {
  placeOrder(toppings, function(order) {
    collectOrder(order, function(pizza) {
      eatPizza(pizza);
    }, failureCallback);
  }, failureCallback);
}, failureCallback)
```

此时还有一个问题，就是对于每个回调函数，都要重复写一个对应的 catch 失败的函数，显然是非常冗余的，因此，ES6提出了<font color=#f07c82>[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)</font>方法！

# Promise

Promise，英语单词，承诺，就像它的名称一样，Promise对象一定会实现它的诺言，也就是执行的结果无论成功与失败，都会给你一个答复。

Promise是一个构造器，可以生成Promise对象，是ES6异步编程的解决方案。

```javascript
let promise = new Promise(function(resolve, reject) {
    if(/*异步执行成功*/){
        resolve(res);
    }else{
        reject(new Error('error'));
    }
})
```

Promise在异步过程中一共有三个状态，**pending**等待结果状态,**fullfilled**的执行成功状态以及**rejected**拒绝状态。

三个状态之间转换关系为：
> pending状态 -> fullfilled 状态
> pending状态 -> rejected 状态

pending无论转向哪两个状态都是resloved(已完成)状态。

一般resolved状态表示fullfilled状态，这是习惯的写法。

## Promise.then()

then()方法是Promise原型对象的方法，Promise.prototype.then()。该方法接受两个参数， promise.then(onFulfilled, onRejected)，一个是异步函数执行成功的处理函数，一个是执行失败的处理函数。但是错误处理函数最好在处理语句的最后用Promise.catch()方法捕获（后面会讲到），因为可提高代码的可读性。


**then()方法的onFulfilled传参要求如下：**
1. **必须是函数，不是函数被忽略。**

```javascript
let promise = new Promise((resolve, reject) =>resolve('你好'));

promise.then('JS').then((result) =>console.log(result));

// 返回 你好，最开始的JS被忽略
```

2. 其参数必须是上个then()或Promise函数所返回。

```javascript
let promise = Promise.resolve('Promise返回的');
let ordinal = ()=>'仅仅函数返回的';
promise.then((result) =>console.log(result)).then(ordinal).then((result) =>console.log(result));

// Promise返回的
// 仅仅函数返回的
```

3. 如果函数没有返回值，则返回 undefined

```javascript
let ordinal = ()=>'仅仅函数返回的';
Promise.resolve().then(function(){
    ordinal();
}).then((result) =>console.log(result));
// undefined
```

4. then()方法只执行一次。


由上规则的值，then()存在链式法则，一系列异步操作可以通过一系列的then()方法连接。

```javascript
chooseToppings()
.then(toppings => placeOrder(toppings))
.then(order => collectOrder(order))
.then(pizza => eatPizza(pizza))
.catch(failureCallback);

// 或者
chooseToppings().then(placeOrder).then(collectOrder).then(eatPizza).catch(failureCallback);
```

## Promise.catch

catch()方法是Promise原型对象的方法，Promise.prototype.catch()，用来捕获错误。

> 最佳实战：永远在then()调用链的最后使用，可以捕获异步操作的任意步骤出现的错误。

```javascript
chooseToppings().then(placeOrder).then(collectOrder).then(eatPizza).catch(failureCallback);
```

## Promise.finally

finally()方法是Promise原型对象的方法，Promise.prototype.finally()，用来最异步操作执行后（无论成功还是失败）都执行的代码。

```javascript
chooseToppings().then(placeOrder).then(collectOrder).then(eatPizza).catch(failureCallback).finally(server.stop);
```

## Promise.all

可以令多个Pormise并行执行，等所有的Promise都执行完毕，然后按照Promise的顺序返回一个由结果构成的Array。

```javascript
let p1 = new Promise((resolve, reject) => {
  resolve('成功了')
})

let p2 = new Promise((resolve, reject) => {
  resolve('success')
})

let p3 = Promse.reject('失败')

Promise.all([p1, p2]).then((result) => {
  console.log(result)               //['成功了', 'success']
}).catch((error) => {
  console.log(error)
})

Promise.all([p1,p3,p2]).then((result) => {
  console.log(result)
}).catch((error) => {
  console.log(error)      // 失败了，打出 '失败'
})
```

## Promise.resolve

Promise.resolve()将现有对象转为Promise对象。

```javascript
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
```

Promise的参数分为四种情况:

1. 参数是Promise实例，则依旧返回Promise实例。

2. 参数是一个 thenable 对象，则将该对象转为 Promise 对象，并立即执行。

```javascript
let thenable = {
  then: function(resolve, reject) {
    resolve(42);
  }
};

let p1 = Promise.resolve(thenable);
p1.then(function(value) {
  console.log(value);  // 42
});
```

3. 如果参数是一个原始值，或者是一个不具有then方法的对象，则Promise.resolve方法返回一个新的 Promise 对象，状态为resolved。

```javascript
const p = Promise.resolve('Hello');

p.then(function (s){
  console.log(s)
});
// Hello
```

4. 不带有任何参数

Promise.resolve方法允许调用时不带参数，直接返回一个resolved状态的 Promise 对象。
```javascript
setTimeout(function () {
  console.log('three');
}, 0);

Promise.resolve().then(function () {
  console.log('two');
});

console.log('one');

// one
// two
// three
```

**注意**

立即resolve的 Promise 对象是在事件循环中当前宏任务的后面添加微任务，因此需要等当前宏任务结束后再执行。

```javascript
Promise.resolve().then(() => console.log(2)).then(() => console.log(3));
console.log(1); // 1, 2, 3
```