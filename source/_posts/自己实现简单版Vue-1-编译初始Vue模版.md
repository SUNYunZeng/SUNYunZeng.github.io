---
title: 自己实现简单版Vue--1. 编译初始Vue模版
comments: true
toc: true
date: 2020-01-29 19:15:05
categories: Web前端
tags: Vue
---

# 前言

Vue的底层到底是怎么实现的呢？

通过手写简单的示例来学习Vue框架的运行机制。

Vue是MVVM框架，其实就是MVC框架在前端的体现，其中的控制器(Controller)由View MOdel(VM)代替。

简单来说，数据更新视图，以及视图更新影响数据这两步操作或者是双向绑定的过程由VM来执行。

而Vue就是一个VM。

Vue的可以说是开箱即用，它的使用非常简单，如下所示：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <h2>{{person.name}} -- {{person.age}}</h2>
        <h3>{{person.fav}}</h3>
        <h3>{{msg}}</h3>
        <div v-text='msg'></div>
        <div v-text='person.name'></div>
        <div v-html='htmlStr'></div>
        <input type="text" v-model='msg'>
        <button v-on:click="handleClick">v-on:click</button>
        <button @click="handleClick">@click</button>
        <a v-bind:href="https://www.bilibili.com/video/av80611222?p=3">v-bind:href</a>
    </div>
    <script src="./Vue.js"></script>
    <script>
        let vm = new Vue({
            el: '#app',
            data: {
                person: {
                    name: '海贼——王路飞',
                    age: 18,
                    fav: '宝藏'
                },
                msg: '最简单的插值',
                htmlStr: '这是v-html'
            },
            methods:{
                handleClick: function(){
                    console.log('这是一个处理点击事件的方法');
                }
            }
        });
    </script>

</body>

</html>
```

通过 new Vue 新建一个Vue实例，并将元素节点与该实例通过<font color=#f07c82> el </font> 实现双向绑定，之后h5中的元素内容跟对象数据就融为一体，开发者的关注重点可以集中在代码逻辑的设计，而不是繁琐的页面与数据绑定问题。

# Vue模版的编译

第一步是将H5中的Vue模版进行编译，使得浏览器能够正确展示对应数据。

其中包括这几部分的编译：

1. 文本模版中插值表达式的编译
    也就是双括号中的值能够正确表达出来，例如{{person.name}}

2. 元素节点中模版表达式的编译
    例如 v-text、v-model、v-html、 v-on:click、 v-bind等

## Mvue入口类设计

Mvue类接收一个参数对象作为初始输入，然后利用<font color=#f07c82>Compiler</font>类对模版进行编译及渲染。

```javascript
class MVue {
    constructor(options) {
        // 初始元素与数据通过options对象绑定
        this.$el = options.el;
        this.$data = options.data;
        this.$options = options;
        // 通过Compiler对象对模版进行编译，例如{{}}插值、v-text、v-html、v-model等Vue语法
        if (this.$el) {
            new Compiler(this.$el, this);
        }
    }

}
```

## Complier编译类设计

```javascript
const compileUtil = {
    getValue(expr, vm){
        // 处理 person.name 这种对象类型，取出真正的value
        return expr.split('.').reduce((data,currentVal)=>{
            return data[currentVal];
        }, vm.$data)
    },
    text(node, expr, vm) {
        let value;
        if(expr.indexOf('{{')!==-1){
            value = expr.replace(/\{\{(.+?)\}\}/g, (...args)=>{
                return this.getValue(args[1], vm);
            });
        }else{
            value = this.getValue(expr, vm);
        }
        this.updater.textUpdater(node, value);  
    },
    html(node, expr, vm) {
        const value = this.getValue(expr, vm);
        this.updater.htmlUpdater(node, value);
    },
    model(node, expr, vm) {
        const value = this.getValue(expr, vm);
        this.updater.modelUpdater(node, value);
    },
    on(node, expr, vm, detailStr) {
        let fn = vm.$options.methods && vm.$options.methods[expr];
        node.addEventListener(detailStr,fn.bind(vm), false);
    },
    bind(node, expr, vm, detailStr){
        // v-on:href='...' => href='...'
        node.setAttribute(detailStr, expr);
    },
    // 视图更新函数
    updater: {
        textUpdater(node, value) {
            node.textContent = value;
        },
        htmlUpdater(node, value){
            node.innerHTML = value;
        },
        modelUpdater(node, value){
            node.value = value;
        }
    }

}

// 编译HTML模版对象
class Compiler {
    constructor(el, vm) {
        this.el = this.isElementNode(el) ? el : document.querySelector(el);
        this.vm = vm;
        // 1. 将预编译的元素节点放入文档碎片对象中，避免DOM频繁的回流与重绘，提高渲染性能
        const fragments = this.node2fragments(this.el);
        // 2. 编译模版
        this.compile(fragments);
        // 3. 追加子元素到根元素
        this.el.appendChild(fragments);
    }
    compile(fragments) {
        // 1.获取子节点
        const childNodes = fragments.childNodes;
        // 2.递归循环编译
        [...childNodes].forEach(child => {
            // 如果是元素节点
            if (this.isElementNode(child)) {
                this.compileElement(child);
            } else {
                // 文本节点
                this.compileText(child);
            }
            //递归遍历
            if(child.childNodes && child.childNodes.length){
                this.compile(child);
            }
        })
    }
    compileElement(node) {
        let attributes = node.attributes;
        // 对于每个属性进行遍历编译
        // attributes是类数组，因此需要先转数组
        [...attributes].forEach(attr => {
            let {name,value} = attr; // v-text="msg"  v-html=htmlStr  type="text"  v-model="msg"
            if (this.isDirector(name)) { // v-text  v-html  v-mode  v-bind  v-on:click v-bind:href=''
                let [, directive] = name.split('-');
                let [compileKey, detailStr] = directive.split(':');
                // 更新数据，数据驱动视图
                compileUtil[compileKey](node, value, this.vm, detailStr);
                // 删除有指令的标签属性 v-text v-html等，普通的value等原生html标签不必删除
                node.removeAttribute('v-' + directive);
            }else if(this.isEventName(name)){
                // 如果是事件处理 @click='handleClick'
                let [, detailStr] = name.split('@');
                compileUtil['on'](node, value, this.vm, detailStr);
                node.removeAttribute('@' + detailStr);
            }

        })

    }
    compileText(node) {
        // 编译文本中的{{person.name}}--{{person.age}}
        const content = node.textContent;
        if(/\{\{(.+?)\}\}/.test(content)){
            compileUtil['text'](node, content, this.vm);
        }
    }
    isEventName(attrName){
        // 判断是否@开头
        return attrName.startsWith('@');
    }
    isDirector(attrName) {
        // 判断是否为Vue特性标签
        return attrName.startsWith('v-');
    }
    node2fragments(el) {
        // 创建文档碎片对象
        const f = document.createDocumentFragment();
        let firstChild;
        while (firstChild = el.firstChild) {
            f.appendChild(firstChild);
        }
        return f;
    }
    isElementNode(node) {
        // 元素节点的nodeType属性为 1
        return node.nodeType === 1;
    }
}
```