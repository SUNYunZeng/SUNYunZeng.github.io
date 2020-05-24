---
title: JS中的设计模式
comments: true
toc: true
date: 2020-05-08 15:33:12
categories: Web前端
tags: JavaScript
---

# 分时函数

对于需要频繁添加大量DOM结构的操作，如果不设计一定的策略，浏览器在短时间渲染这个多DOM结构，会造成浏览器的卡顿。

此时可以设计分时函数，在指定时间间隔内，执行指定次数的函数，添加指定数量的DOM结构。

例如有一个好友列表，需要动态渲染其中成百上千的列表，此时可以采用分时函数。

```javascript
let timeChunk = function(arr, fn, count, time){
    let start = function(){
        for(let i=0; i<Math.min(count || 1, arr.length); i++){
            let content = arr.shift();
            fn(content);
        }
    }

    return function(){
        let t = setInterval(function(){
            if(arr.length===0){
                clearInterval(t);
                t=null;
            }else{
                start();
            }
        }, time);
    }
}

let ls = [];

for(let i=0; i<100; i++){
    ls.push(i);
}

let renderFriendList = timeChunk(ls, function(content){
    let div = document.createElement('div');
    div.innerHTML = content;
    document.body.appendChild(div);
}, 8, 200);

renderFriendList();
```