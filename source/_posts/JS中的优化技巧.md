---
title: JS中的优化技巧
comments: true
toc: true
date: 2020-07-23 15:47:29
categories: Web前端
tags: JavaScript
---

# 使用 Promise.all 同时触发多个不互相依赖的异步请求

对于互不依赖的多个Promise任务，可以使用 Promise.all 方法使这些方法并行执行，而不是串行执行，这样可以大量的节省任务执行时间。

**举例**

```javascript
function getData1(){
  return new Promise((resolve, reject)=>{
    setTimeout(()=>resolve(1), 10000);
  })
}

function getData2(){
  return new Promise((resolve, reject)=>{
    setTimeout(()=>resolve(2), 20000);
  })
}

async function oneByOneAsyncFunc(){
  const start_time = new Date();
  const a = await getData1();
  const b = await getData2();
  console.log({
    a,
    b,
  });
  const end_time = new Date();
  console.log(end_time.getSeconds()-start_time.getSeconds());
}

async function twoAsyncFunc(){
  const start_time = new Date();
  let [a, b] = await Promise.all([getData1(), getData2()]);
  console.log({
    a,
    b,
  });
  const end_time = new Date();
  console.log(end_time.getSeconds()-start_time.getSeconds())
}

twoAsyncFunc() // 执行时间为 20 秒

oneByOneAsyncFunc() // 执行时间为 30 秒

```

[Promise 的一些知识](http://sunyunzeng.com/JS%E5%BC%82%E6%AD%A5%E5%87%BD%E6%95%B0%E5%B0%8F%E7%BB%93/)