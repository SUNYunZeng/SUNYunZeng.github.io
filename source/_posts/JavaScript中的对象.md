---
title: JavaScript中的对象
comments: true
toc: true
date: 2019-11-16 20:09:17
categories:  Web前端
tags: JavaScript
---

# 对象的概念

**JS 中的对象其实是无序属性的集合，属性包括基本数据、函数与对象**，每个属性有属性名，属性名与属性值以键值对的形式保存在对象内部。
```javascript
// JS中的一个对象
var person = {
    name: "syz",
    job: "Software Engineer",
    sayHello: function(){
        console.log("Hello");
    } 
}
```

# 对象的属性

为了JavaScript引擎可以操作对象中的属性，每个属性定义了对应的两种属性类型：

**1. 数据属性。**  **2. 访问器属性。**

## 数据属性

是对一个对象属性值的操作说明，共有四类说明：**configurable、enumerable、writable及value**。

可通过<font color=#f07c82>Object.defineProperty</font>实现不同属性的值操作限定，接收三个参数，第一个是操作对象、第二个是操作对象的属性第三个是对该属性的操作限定。

```javascript
var person = {
    name: "syz",
    job: "Software Engineer",
    age: 24
}

Object.defineProperty(person, "age", {
    /*
    默认true。
    设置为false，则该属性不能被删除，configurable、enumerable、writable(true还可设定为false)不能再设定；
    该属性不能在通过Object.defineProperty()方法修改为访问器属性。
    */
    configurable: true,

     /*
     默认true。
    设置为false，则不能通过for-each方式遍历该属性。
    for(var item in person){
        console.log(item) // 不会输出person的age属性
    }
    */
    enumerable: true, 
    
    /*默认true。
    设置为false，则属性值为可读的，严格模式强行修改会报错，一般模式修改无效。
    */
    writable: true,

     // 设置该属性的值
    value: 25
});
```

## 访问器属性

访问器属性是对访问对象属性值的操作的说明，共四类：**configurable、enumerable、get及set**。

可通过<font color=#f07c82>Object.defineProperty</font>实现不同属性的值操作限定，接收三个参数，第一个是操作对象、第二个是操作对象的属性第三个是对该属性的操作限定。

```javascript
var person = {
    name: "syz",
    job: "Software Engineer",
    _age: 24
}

Object.defineProperty(person, "age", {
    /*
    默认为true。
    设置为false，则该属性不能被删除，该属性不能在通过Object.defineProperty()方法修改为数据属性。
    */
    configurable: true,

     /*
     默认为true。
    设置为false，则不能通过for-each方式遍历该属性。
    for(var item in person){
        console.log(item) // 不会输出person的age属性
    }
    */
    enumerable: true, 
    
    /*设置读取该属性时的返回值。
    注意_age前面有 _ 下划线，表示该属性值只能通过对象的方法访问到，也就是get()方法，如果没有定义相应的方法，
    采用person._age访问会返回undefined。
    如果_age前面不加下划线，而又通过get()函数访问this.age的话，会导致函数调用栈溢出.
    因为person.age -> get.call(person) ->this.age -> person.age -> ...
    */
    get: function(){
        return this._age;
    },

     // 改属性赋值时的操作
    set: function(value){
        this._age = value;
        if(value > 50){
            this.job = "rubbish";
        }
    }
});
```

## 数据属性其它知识

+ 通过<font color=#f07c82>Object.defineProperties()</font>方法一次性为对象的多个属性设置属性类型。

```javascript
var book = {};
Object.defineProperies(book, {
    name: {
        value: "machine learning"
    },
    _year: {
        value: 2019
    },
    edition{
        value: 1
    }
    year: {
        get: function() {
            return this._year;
        },
        set: function(value) {
            if(value>2019){
                this._year = value;
                this.edition += value - this._year;
            }
        }
    }
})
```

+ 通过<font color=#f07c82>Object.getOwnPropertyDescriptor()</font>方法**获得指定对象指定属性的属性类型说明对象。**第一个参数接收查询对象，第二个参数接收对应对象的查询属性的**字符串表示**。该方法只能获取实例对象的属性类型描述对象，对于原型对象，需要调用Person.prototype。

```javascript
var descriptor = Object.getOwnPropertyDescriptor(book, "year");
console.log(descriptor.enumrable); //如果是访问器属性，则返回undefined
console.log(descriptor.value);
console.log(descriptor.writable); //如果是访问器属性，则返回undefined
console.log(descriptor.configurable);
```

# 创建对象

JavaScript中自定义的对象又是怎么创建来的呢？从前面知识，我们了解到，可以通过**构造函数或者对象字面量**的形式创建，如下：
```javascript
// 默认的Object构造函数
var person = new Object({
    name : "syz"
});

// 对象字面量形式
var person = {
    name : "syz"
}
```

首先，通过对象字面量形式创建的对象只能是Obejct类型实例，无法满足特定实例的要求。

然后，再看通过构造函数创建的方式。

## 构造函数

构造函数其实就是函数的一种，**任何函数都可以是构造函数**，只要在函数前面使用 <font color=#f07c82>new</font> 关键字，就使该函数成为了构造函数。

```javascript
// 构造函数通常大写
function Person(name, age) {
    this.age = age;
    this.name = name;
    this.sayHello = function (){
        console.log("Hello");
    }
}
var person = new Person("syz", 12);
```

前面提到，函数其实也是一种**Funcation类型**的实例对象，加入了new关键字后，函数执行四个步骤:

1. 创建一个新对象。
2. 将构造函数的作用域赋给对象，即this的主体换为新对象。
3. 执行构造函数的语句，对新对象执行初始化操作。
4. 返回该对象。

构造函数也可以当做普通函数使用，如下：
```javascript
// 此时函数的作用域是全局执行环境，因此也就是给windows对象赋予了name及age属性
Person("syz", 12);
//通过调用call()方法在指定作用域执行,相当于为person对象赋予了姓名与年龄属性
var person = {};
Person.call(person, "syz", 12);
```

通过构造函数创建的对象实例可以通过关键字 <font color=#f07c82>instanceof</font>来判断其是否为某自定义引用类型的对象。
```javascript
person instanceof Person; // => true
```

## 原型对象

然而，仅通过构造函数创建对象存在弊端。即一些属于类公共部分的属性（尤其是引用类型值）仍需要在每个实例创建过程中重新创建一份副本。
```javascript
function Person(name) {
    this.name = name;
    this.sayHello = function (){
        console.log("Hello");
    }
}

var person1 = new Person("syz");
var person2 = new Person("xj");

console.log(person1.sayHello===person2.sayHello); // =>false
```

因为 sayHello() 方法是 **Function** 类型的实例，尽管其作用在每个对象实例中是相同的，但是在每个对象创建过程中，每个sayHello() 都重新创建一份副本，导致内存资源的浪费。

JS通过构造函数的**原型对象**来解决。

**原型对象其实是构造函数的属性之一**，可以通过 <font color=#f07c82>Person.prototype</font> 访问 Person 构造函数原型对象，其中**保存了 Person 类共享的属性与方法**。

原型对象在**每个函数**的创建过程中自动生成，即每个函数自动生成一个 <font color=#f07c82>prototype</font> 属性，指向一个原型对象，该原型对象自动生成一个 <font color=#f07c82>constructor</font>属性，指向构造函数，其**属性类型是不可枚举的**。

```javascript
function Person(){}
// 为原型兑现赋值
Person.prototype.name = "Nicholas"; 
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer"; 
Person.prototype.sayName = function(){ 
    alert(this.name);
};

/* 还可以用简便的方法为原型对象赋值
Person.prototype = {
    name: "Nicholas",
    age: 29,
    job: "Software Engineer",
    sayName: function(){
        alert(this.name);
    }
    /*由于此时完全重写了系统自动生成的的原型对象，所以constroctor属性不再指向Person，而是Object
     此时再用person.constroctor == Person 就返回false,需要重新定义constroctor的指向
    */
    // constructor: Person, 但是此时的constructor属性时可枚举的，可用下列语句修改属性类型
    Obeject.defineProperty(Person.prototype, "constroctor"{
        enumerable: false,
        value: Person
    });
};

var person1 = new Person(); 
var person2 = new Person(); 

person1.sayName(); //"Nicholas"
var person2 = new Person(); 
person2.sayName(); //"Nicholas" 

alert(person1.sayName == person2.sayName); //true
```
<center>{% asset_img prototype.png%}</center>
<center>各对象之间的关系</center>

在一个实例中访问某个属性，首先在实例本身搜索，如果没找到，再通过内部属性[\[Property]]找到指向的原型对象，再在其内部寻找。

如果实例内部定义了与原型对象中重名的属性，则读取到该属性即停止搜索。

```javascript
function Person(){}
Person.prototype.name = "Nicholas"; 
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer"; 
Person.prototype.sayName = function(){ 
    alert(this.name);
};

var person = new Person();
person.height = 30;
```

+ 相关函数
    - **isPrototypeOf():** 判断某个原型对象是否是某个实例对应的原型对象
    ```javascript
    Person.prototype.isPrototypeOf(person); // => ture
    ```
    - **Object.getPrototypeOf():** 获得某个实例的原型对象
    ```javascript
    var proto = Object.getPrototypeOf(person);
    ```
    - **hasOwnProperty():** 判断某个属性是来自实例还是原型对象，来自实例返回true
    ```javascript
    console.log(persom.hasOwnProperty("height")); //=> true
    ```
    - **in**: 判断某个实例是否包含某个属性，无论是位于实例本身还是原型对象
    ```javascript
    console.log("job" in person); //=> true
    console.log("height" in person); // => true
    ```
    - **for in:** 遍历所有可枚举属性，无论是位于实例本身还是原型对象
    ```javascript
    for(var item in person){
        console.log(item); // name,height,age,job,sayName
    }
    ```
    - **Object.keys():** 以字符串数组形式返回实例本身可枚举的属性
    ```javascript
    console.log(Object.keys(Person.prototype)); //[ 'name', 'age', 'job', 'sayName' ]
    console.log(Object.keys(person)); //[ 'name', 'height' ]
    ```
    - **getOwnPropertyNames():** 以字符串数组形式返回实例本身所有的属性（包括不可枚举）
    ```javascript
    console.log(Object.getOwnPropertyNames(Person.prototype)); //[ 'constructor', 'name', 'age', 'job', 'sayName' ]
    console.log(Object.getOwnPropertyNames(person)); //[ 'name', 'height' ]
    ```

## 混合模式创建对象

通过原型模式可以设置共享的对象类型，通过构造器模式可以为对象的创建赋予初始值，结合二者的混合模式是最常见的创建对象的方法。

```javascript
function Person(name, age){
    this.name = name;
    this.age = age;
}

Person.prototype = {
    constroctor: Person,
    sex: "man"
}

if(typeof Person.sayName != "function"){
    Person.prototype.sayName = function(){
        console.log("Hello, my name is " + this.name);
    };
}

person1 = new Person("syz", 24);
person2 = new Person("xj", 23);

console.log(person1.name); // "syz"
console.log(person2.name); // "xj"
console.log(person1.sex); // "man"
console.log(person2.sex); // "man"
```