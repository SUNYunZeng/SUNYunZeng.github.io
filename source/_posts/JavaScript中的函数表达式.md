---
title: JavaScript中的函数表达式
comments: true
toc: true
date: 2019-11-19 21:19:14
categories: Web前端
tags: JavaScript
---

# 函数表达式

**函数表达式是定义函数的一种方式**，如下：

```javascript
// 函数表达式
var add = function(v1, v2){
    return v1 + v2;
}
// 函数声明
function add(v1, v2) {
    return v1 + v2;
}
```

函数表达式可以创建一个匿名函数，它与JS的很多特性与模式有关，包括闭包、私有变量创建等等。

# 闭包

**闭包指一个函数，该函数有权访问其他函数作用域中的变量。**

```javascript
function createComparisonFunction(propertyName){
    // 闭包
    return function(object1, object2){
        var value1 = object1[propertyName];
        var value2 = object2[propertyName];

        if(value1 < value2){
            return -1;
        }else if(value1 > value2){
            return 1;
        }else{
            return 0;
        }
    }
}

var compare = createComparisonFunction("name");
console.log(compare({name: "syz"}, {name: "xj"}));

// 释放内存
compare = null;
```
放回的匿名函数引用了外包函数createComparisonFunction()的变量propertyName。

需要注意的是，在createComparisonFunction()执行完毕产生比较函数compare()时，其**作用域链**被销毁，但是其**活动对象**(此处指**propertyName**)仍被compare()函数引用，并没有销毁。

闭包实现的具体原理是：
> 首先，每一个执行环境都有一个**变量对象**表示。全局执行环境的变量对象始终存在，而函数内部的变量对象在执行过程中创建，执行完毕销毁。**createComparisonFunction()**函数执行过程，首先创建一个包含全局变量对象的作用域链，并被内部属性[\[Scope]]引用，其中的**参数、变量与方法**都作为**活动对象**(该函数的变量对象)被推入作用域链的顶端，构成**该函数的作用域链**。作用域链提供一个变量对象的引用链，通过它可以访问链上的变量。

> 然后，内部函数的创建，首先创建一个包含全局变量对象的作用域链。如果内部函数引用了外部函数的变量，即**闭包**，那么内部函数的作用域链会添加**外部函数的活动对象**，最后再将本函数的活动对象推入作用域链顶端。

> 最后，外部函数createComparisonFunction()执行完毕，本来所有作用域链与活动对象全部销毁，但由于内部函数的引用，导致只销毁作用域链，而**活动对象继续被内部函数引用，直到内部函数被销毁。**

{% asset_img % bibao.png %}

# 匿名函数中的this对象

**要熟知函数的this对象指向其对应的执行环境。**

```javascript
// 例子1
var object = {
    name: "this indicator",
    getName: function(){
        return function(){
            return this.name;
        }
    }
}
/* 
因为调用函数getName()返回一个内部函数，而此内部函数的执行环境为全局执行环境,
尽管其作用域链存在外部函数的this对象，但是作用域链的顶端this对象指向全局执行环境,
所以返回 windows
*/
console.log(object.getName()()); // windows

// 例子2
var object = {
    name: "this indicator",
    getName: function(){
        var that = this;
        return function(){
            return that.name;
        }
    }
}
// 指定匿名函数的this对象就是指向外部函数的this对象，因此返回 this indicator
console.log(object.getName()()); // this indicator
```

# 模拟块级作用域

由于JS不存在块级作用域，因此**if、for**语句中声明的变量存在于全局变量中，会导致内存的溢出及命名的冲突。

可以通过**匿名函数的方式模拟模拟块级作用域**，将生命的变量**随匿名函数的生命周期绑定。**

```javascript
(function() {
    for(var i = 0; i <10; i++){
        console.log(i);
    }
})(); // 表示立即执行该函数，如果匿名函数没有被()包裹，则报错。
```

# 自定义类型的私有变量

JS的所有属性没有私有概念。但是函数内的变量是局部的，外部不可访问的。
可**在构造函数或私有作用域中声明私有变量与方法，在匿名函数中引用外部函数的私有变量，并提供可访问这些方法的特权函数，实现自定义类型的私有变量。**

## 构造函数中创建私有变量

```javascript
function Person(){
    // 私有变量与方法
    var name = "syz";
    function getGender(){
        return "man";
    }
    // 特权方法获取私有变量
    this.getInfo = function(){
        name = "xj";
        return getGender();
    }
}
```

还可以通过匿名函数实现私有变量的**不可修改与直接读取的设定**。

```javascript
function Person(name){
    
    // 保护私有变量，此时Person构造函数的变量name通过闭包访问，形成了每个成员的保护变量
    this.getName = function(){
        return name;
    }
    this.setName = function(value){
        name = value;
    }
}
```

## 静态私有变量

采用构造函数声明私有变量对每个实例都是唯一的，会造成代码的重复。

对属于类的公共的属性与方法，可以通过静态私有变量的方式私有化。

```javascript
// 通过私有作用域实现私有变量的保存

(function(){
    // 私有变量
    var name = "";
    // 全局构造函数
    Person = function(value){
        name = value;
    }

    // 通过原型实现静态私有变量
    Person.prototype.getName = function(){
        return name;
    }
    Person.prototype.setName = function(value){
        name = value;
    }
})();

var person1 = new Person("syz");
console.log(person1.getName()); // syz

var person2 = new Person("xj");
console.log(person1.getName()); // xj
console.log(person2.getName()); // xj
```
# 单例的私有变量

JS中的单例很简单。
```javascript
var singleton = {
    description: "I am a singleton";
}
```
## 模块模式

单例可以通过**模块模式**的方式实现变量的私有化。基本思想是**通过将私有变量与方法保存在外部匿名函数的活动对象中，然后返回一个具有特权访问函数的匿名对象。**

```javascript
var application = function() {
    // 私有变量
    var components = new Array();
    // 初始化
    components.push(new BaseComponent());

    // 放回单例，单例内通过特权方法访问私有变量
    return {
        getComponentCount : function(){ 
            return components.length;
        },
        registerComponent : function(component){ 
            if (typeof component == "object"){ 
                components.push(component);
            } 
        }
    }
}(); // 立即执行函数
```

## 增强模块模式
如果想创建指定类型的单例，并为其添加私有变量及增加属性与方法，可采用**增强模块模式**。基本思想与模块模式差不多，只不多不返回匿名对象，而是先创建指定类型的单例对象，然后为该对象添加特权访问函数、增强属性与方法，然后返回。

```javascript
var application = function(){ 
    //私有变量和函数
    var components = new Array(); 
    //初始化
    components.push(new BaseComponent());
    //创建 application 的一个局部副本 
    var app = new BaseComponent(); 
    //公共接口
    app.getComponentCount = function(){ 
        return components.length;
    };
    app.registerComponent = function(component){ 
        if (typeof component == "object"){ 
            components.push(component);
        }
    }; //返回这个副本
    return app;
}();
```