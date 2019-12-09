---
title: JavaScript中的继承
comments: true
toc: true
date: 2019-11-18 20:17:35
categories: Web前端
tags: JavaScript
---

# 浅述JS继承

JS作为一门不是很严谨的OO语言，由于其函数没有签名，因此不能实现签名继承的接口形式，却可实现属性与方法的完全继承。

JS的继承方式有六种：**原型链继承、构造函数继承、组合继承、原型式继承、寄生式继承及寄生组合继承。**

## 原型链继承

在JS对象一章讲到了构造函数的**原型对象**，如果一个构造函数的原型对象是另一个构造函数的实例，那么通过该构造函数创建的实例就继承了原型对象对应构造函数的全部属性与方法。

```javascript
function SuperType(name){
    this.name= name;
    this.color = ["red", "green", "blue"];
}

SuperType.prototype.sayHello = function(){
    console.log("Hello " + this.name);
}

function SubType(name,age){
    this.name=name;
    this.age=age;
}

// 原型链继承
SubType.prototype = new SuperType();

var sub = new SubType("sg", 26);
sub.sayHello(); // Hello sg

var sub1 = new SubType("syz", 24);
var sub2 = new SubType("xj", 22);
sub1.color.push("black");
console.log(sub2.color); // [ 'red', 'green', 'blue', 'black' ]
```

不足：1. 对于父类的引用类型，在子类中是共享的，因为子类原型对象指向同一父类实例，无法实现与子类的单独绑定。 2. 没有办法在不影响子类的情况下为父类构造函数赋予初始值。

## 构造函数继承

在子类构造函数中，调用父类构造函数方法，可以实现父类属性（尤其是引用类型）与子类的单独绑定。

```javascript
function SuperType(name){
    this.name= name;
    this.color = ["red", "green", "blue"];
}

SuperType.prototype.sayHello = function(){
    console.log("Hello " + this.name);
}

// 构造函数继承
function SubType(name,age){
    // 在SubType执行环境作用域执行父类构造函数的初始化函数
    SuperType.call(this, name);
    this.age=age;
}

var sub = new SubType("sg", 26);
// 原型对象是通过内部指针访问，通过执行函数的方式无法访问
sub.sayHello() //TypeError: subType.sayHello is not a function

var sub1 = new SubType("syz", 24);
var sub2 = new SubType("xj", 22);
sub1.color.push("black");
sub2.color.push("yellow");
console.log(sub1.color); //[ 'red', 'green', 'blue', 'black' ]
console.log(sub2.color); //[ 'red', 'green', 'blue', 'yellow' ]
```

不足：无法复用父类原型对象中公共的属性与方法。

## 组合继承

组合继承就是综合原型链与构造函数的继承方式，结合二者各自的优点，可以继承父类构造函数与原型对象中的属性与方法，且可以实现父类引用类型数据与子类的单独绑定。

```javascript
function SuperType(name){
    this.name= name;
    this.color = ["red", "green", "blue"];
}

SuperType.prototype.sayHello = function(){
    console.log("Hello " + this.name);
}

function SubType(name,age){
    // 在SubType执行环境作用域执行父类构造函数的初始化函数
    SuperType.call(this, name); // 第二次调用
    this.age=age;
}

SubType.prototype = new SuperType(); //第一次调用
// 防止指向父类的构造器
SubType.prototype.constructor = SubType;

var sub = new SubType("sg", 26);
// 原型对象是通过内部指针访问，通过执行函数的方式无法访问
sub.sayHello() // Hello sg
```
不足： 重复调用了两次的父类的构造函数，导致子类原型对象中存储了重复的无用的数据。

## 原型式继承

如果只想使得某些类表现得像某个类，可以采用原型式继承。即使这些类的原型对象指向同一个对象。
```javascript
var Person = {
    name: "syz", 
    friends: ["mzp", "wjh"]
}

// 原型继承函数
function object(o){
    function F(){}
    F.prototype = o;
    return new F;
}

var person1 = object(Person);
person1.name = "John";
person1.friend.push("lili");
var person2 = object(Person);
person1.name = "gad";
person1.friend.push("uzi");

console.log(person1.name); // John
console.log(person2.name); // gad
console.log(person1.friends); // [ 'mzp', 'wjh', 'lili', 'uzi' ]
console.log(person2.friends); // [ 'mzp', 'wjh', 'lili', 'uzi' ]
```

ECMAScript5规范了原型式继承的方式，即采用<font color=#f07c82>Object.create()</font>方法，参数数即要继承的包含共用的属性与方法的对象，第二个参数为可选参数Object.defineProperties()方法的第二个参数格式相同：每个属性都是通过自己的描述符定义的。

Object.create()作用与object()方法相同。

```javascript
var Person = {
    name: "syz", 
    friends: ["mzp", "wjh"]
}
var person1 = Object.create(Person, {
    name: {
        value: "xj"
    }
});
console.log(person1.name); // xj
console.log(person1.friends); // [ 'mzp', 'wjh' ]
```

## 寄生继承

寄生继承组合了原型式继承与工厂方法，或者是装饰器模式，可以对原型示例添加装饰方法。

```javascript
var Person = {
    name: "syz", 
    friends: ["mzp", "wjh"]
}

function createPerson(_name_){
    var person = Object.create(Person, {
        name: {
            value: _name_
        }
    });
    person.sayHi = function(){
        console.log("Hi");
    }
    return person;
}

var person = createPerson("xj");
person.sayHi(); // Hi
console.log(person.name); // xj
```

## 寄生组合继承

是JS继承的**最优解**，通过寄生继承减少了组合继承中多余的父类构造函数调用产生的子类原型对象属性重复的问题。

对于子类的原型对象，采用寄生继承方式继承自父类的原型对象。
```javascript
function SuperType(name){
    this.name= name;
    this.color = ["red", "green", "blue"];
}

SuperType.prototype.sayHello = function(){
    console.log("Hello " + this.name);
}

function inheritPrototype(subType, superType){
    subType.prototype = Object.create(superType.prototype);
    subType.prototype.constructor = subType;
}

function SubType(name,age){
    SuperType.call(this,name);
    this.age=age;
}

inheritPrototype(SubType,SubType);

var sub = new SubType("syz", 12);
console.log(sub.name); // syz
console.log(sub.age); // 12
console.log(sub.color); // [ 'red', 'green', 'blue' ]
sub.sayHello(); // Hello syz
```

**完美！**