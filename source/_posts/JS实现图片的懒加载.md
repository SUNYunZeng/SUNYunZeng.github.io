---
title: JS实现图片的懒加载
comments: true
toc: true
date: 2020-03-16 21:37:26
categories: Web前端
tags: JavaScript
---

# 场景需求

在长博客、商品网站上经常见到大量的图片，而同时加载如此多的图片会导致服务器响应慢、页面卡顿，用户体验不佳。

而**图片懒加载**就是应对这个痛点。图片懒加载技术就是根据图片在可视区域的位置对图片进行逐步加载，利用 DOM对象元素属性、分流等技术实现。

# 实现原理

## 步骤一   隐藏懒加载图片的 src 属性

我们首先需要确定懒加载的图片资源，对于 <font color=#f07c82>img</font> 标签，首先把<font color=#f07c82>src</font> 属性用自定义的属性如 <font color=#f07c82>data-src</font> 代替，等到图片需要加载时我们将  <font color=#f07c82>data-src</font> 的网址赋予 <font color=#f07c82>src</font> 属性即可。

```html
<img data-src='./dzq.jpg' alt="dzq">
```

## 步骤二   判断待加载图片是否位于视窗范围内

DOM 元素拥有一个方法 <font color=#f07c82>element.getBoundingClientRect()</font>，可以获取该元素的大小及其相对于视口的位置，方法返回的是一个 DOMRect 对象，其 left, top, right, bottom 属性解释了相对于视口位置，如下所示。

<center>{% asset_img rect.png %}</center>

然后利用 **<font color=#f07c82>img.getBoundingClientRect().top < document.documentElement.clientHeight</font>** 对图片位置进行判断。

## 步骤三   对加载到视口内的元素进行显示

通过对 <font color=#f07c82>windows.scroll</font> 进行监听，然后 img 元素新建 <font color=#f07c82>src</font> 属性，并将 <font color=#f07c82>data-src</font> 的地址赋予该属性。

# 优化

而我们在监听<font color=#f07c82>windows.scroll</font> 事件过程中，对于 <font color=#f07c82>img</font> 元素的操作一致在进行，这就浪费了很多计算资源。为了绕开这个问题，我们使用<font color=#f07c82>节流</font>技术手段。

## 节流

对于高频事件，我们希望**处理函数仅在固定的频率响应**。例如，对于窗口滑动事件，可能每 20ms 就触发一次，而我们希望每 200ms 处理函数只响应一次，这样就节约了很多不必要的计算资源。

```javascript
// 节流函数定义
function throttle(fn, wait){
    let timer;
    return function(){
        let that = this;
        let args = arguments;
        if(!timer){
            timer = setTimeOut(function(){
                fn.apply(that, args);
                timer = null;
            },wait)
        }
    }
}

// 节流函数使用
widow.addEventListener("scroll", throttle(showImg, 200));
```

## 防抖

另一个类似的技术手段就是防抖，是在**高频事件触发后一定时间内使处理函数响应**。它与节流函数的区别是，如果高频事件一直触发，那么处理函数一直得不到响应。

```javascript
function debounce(fn, wait){
    let timer;
    return function(){
        let that = this;
        let args = arguments;
        if(!timer){
            clearTimeOut(timer);
        }
        timer = setTimeOut(function(){
            fn.apply(that, args);
        }, wait);
    }
}
```

# demo

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<style>
    html, body{
        height: 100%;
        width: 100%;
        padding: 0;
        margin: 0;
    }
</style>
<body>
    <div id="upContext" style="height: 100%; width: 100%; position: relative; background-color: aquamarine;">?</div>
    <div style="display:flex; position: relative; align-items: center; justify-content: center;">
        <img d-src="./dzq.jpg" alt="图片" id="img1">
    </div>
</body>
<script>
    let img = document.getElementById('img1');
    let showImg = function(){
        if(img.getBoundingClientRect().top < document.documentElement.clientHeight){
            img.setAttribute('src', img.getAttribute('d-src'));
        }
    }
    function throttle(fn, wait){
        let timer;
        return function(){
            let that = this;
            let args = arguments;
            if(!timer){
                timer = setTimeout(function(){
                    fn.apply(that, args);
                    timer = null;
                }, wait);
            }
        }
    }

    window.addEventListener("scroll", throttle(showImg, 200));
    
</script>
</html>
```