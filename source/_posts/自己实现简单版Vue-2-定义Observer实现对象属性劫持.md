---
title: 自己实现简单版Vue--2. 实现数据绑定视图
comments: true
toc: true
date: 2020-01-31 21:33:24
categories: Web前端
tags: Vue
---

# 利用`Object.defineProperty()`方法实现数据的监听

Object.defineProperty()方法可以具体参考链接：http://sunyunzeng.cn/JavaScript%E4%B8%AD%E7%9A%84%E5%AF%B9%E8%B1%A1/#%E8%AE%BF%E9%97%AE%E5%99%A8%E5%B1%9E%E6%80%A7

该方法可以定义对象数据在访问操作时的一些约定。

1. 定义 Observer 对象

```javascript
class Observer{
    constructor(data){
        this.observe(data);
    }
    // data是一个对象，可能嵌套其它对象，需要采用递归遍历的方式进行观察者绑定
    observe(data){
        if(data && typeof data === 'object'){
            Object.keys(data).forEach(key =>{
                this.defineReactive(data, key, data[key]);
            })
        }
    }
    // 通过 object.defineProperty方法对对象属性进行劫持
    defineReactive(obj, key, value){
        // 递归观察
        this.observe(value);
        Object.defineProperty(obj, key, {
            enumerable: true,
            configurable: false,
            get(){
                return value;
            },
            // 采用箭头函数在定义时绑定this的定义域
            set: (newVal)=>{
                if(newVal !== value){
                    this.observe(newVal);
                    value = newVal;
                    console.log(newVal);
                }
            }
        })
    }
}
```

2. 利用Observer对象对数据进行劫持

```javascript
class MVue {
    constructor(options) {
        // 初始元素与数据通过options对象绑定
        this.$el = options.el;
        this.$data = options.data;
        this.$options = options;
        // 通过Compiler对象对模版进行编译，例如{{}}插值、v-text、v-html、v-model等Vue语法
        if (this.$el) {
            // 1. 编译模版
            new Compiler(this.$el, this);
            // 2. 创建观察者，观察数据
            new Observer(this.$data);
        }
    }
}
```

3. 定义Dep容器及Watcher对象对数据变化进行监听

```javascript
class Watcher{
    // 通过回调函数实现更新的数据通知到视图
    constructor(expr, vm, cb){
        this.expr = expr;
        this.vm = vm;
        this.cb = cb;
        this.oldVal = this.getOldVal();
    }
    // 获取旧数据
    getOldVal(){
        // 在利用getValue获取数据调用getter()方法时先把当前观察者挂载
        Dep.target = this;
        console.log(Dep.target);
        const oldVal = compileUtil.getValue(this.expr, this.vm);
        // 挂载完毕需要注销，防止重复挂载 (数据一更新就会挂载)
        Dep.target = null;
        return oldVal;
    }
    // 通过回调函数更新数据
    update(){
        const newVal = compileUtil.getValue(this.expr, this.vm);
        if(newVal !== this.oldVal){
            this.cb(newVal);
        }
    }
}

// Dep类存储watcher对象，并在数据变化时通知watcher
class Dep{
    constructor(){
        this.watcherCollector = [];
    }
    // 添加watcher
    addWatcher(watcher){
        console.log('观察者', this.watcherCollector);
        this.watcherCollector.push(watcher);
    }
    // 数据变化时通知watcher更新
    notify(){
        this.watcherCollector.forEach(w=>w.update());
    }
}
```

4. 在Observer中绑定Dev

```javascript
class Observer{
    // ... 省略
    defineReactive(obj, key, value){
        // ... 省略
        const dep = new Dep();
        Object.defineProperty(obj, key, {
            // ... 省略
            get(){
                // 订阅数据变化时，往Dev中添加观察者
                Dep.target && dep.addWatcher(Dep.target);
                return value;
            },
            // 采用箭头函数在定义时绑定this的定义域
            set: (newVal)=>{
                // ... 省略
                // 通知watcher数据发生改变
                dep.notify();
            }
        })
    }
}
```

5. 在编译工具中绑定Watcher
```javascript
const compileUtil = {
    // ... 省略
    getContent(expr, vm){
        // {{person.name}}--{{person.age}}
        // 防止修改person.name使得所有值全部被替换
        return expr.replace(/\{\{(.+?)\}\}/g, (...args)=>{
            return this.getValue(args[1], vm);
        });
    },
    text(node, expr, vm) {
        let value;
        if(expr.indexOf('{{')!==-1){
            value = expr.replace(/\{\{(.+?)\}\}/g, (...args)=>{
                // text的 Watcher应在此绑定，因为是对插值{{}}进行双向绑定
                // Watcher的构造函数的 getOldVal()方法需要接受数据或者对象，而{{person.name}}不能接收
                new Watcher(args[1], vm, ()=>{
                    this.updater.textUpdater(node, this.getContent(expr, vm));
                });
                return this.getValue(args[1], vm);
            });
        }else{
            value = this.getValue(expr, vm);
        }
        this.updater.textUpdater(node, value);  
    },
    html(node, expr, vm) {
        let value = this.getValue(expr, vm);
        // html对应的 Watcher
        new Watcher(expr, vm, (newVal)=>{
            this.updater.htmlUpdater(node, newVal);
        })
        this.updater.htmlUpdater(node, value);
    }
```

6. 在MVue类的构造函数中绑定Observer

```javascript
class MVue {
    constructor(options) {
        this.$el = options.el;
        this.$data = options.data;
        this.$options = options;
        if (this.$el) {
            // 1. 创建观察者
            new Observer(this.$data);
            // 2. 编译模版
            new Compiler(this.$el, this);
        }
    }
}
```

# 项目地址

https://github.com/SUNYunZeng/ImitateVue