---
title: JavaScript基础知识点
comments: true
toc: true
date: 2019-11-13 22:48:02
categories: Web前端
tags: JavaScript
---

# 为啥“又”学JavaScript？

那年头发还多，正是大四上学期。提前联系了导师，由于项目原因，第一次接触到JS。

然后，看了两周左右的入门书籍，入了前端的坑。

结果，自己觉得前端没啥技术含量，跑回去学后端开发，也就是Java开发的一套东西。

看到数据库、并行化编程部分，心态崩了。

自己还是喜欢前端开发。

又滚回来重新学习。

看到网页在自己面前展示，**真TM有意思**。

# JavaScript这门语言

**JavaScript**很有趣，比**Java**有趣多了。

**Java**好比是一个严谨的大叔，什么都严格要求，准确但无趣。

**JavaScript**好比是一个热血青年，喜欢打篮球，生动有趣。

不过JavaScript也不老了，1995年诞生。当时叫这个名字还是蹭Java的热度。。。

JavaScript由三剑客组成：**ECMAScript、DOM、BOM**。

## ECMAScript

ECMAScript是ECMA-262标准定义的一种语言规范，规定了一门语言的5类部分：**类型、语句、关键字、保留字、操作符及对象。**

ECMAScript没有定义任何语言运行的环境要求，只是说明，你按照我这个规范，可以实现一种编程语言。不像Java，规定了虚拟机作为它的运行环境。

浏览器可以作为ECMAScript语言的运行环境，当然Node、Abode Flash也都是。

<font color=#f07c82>JavaScript</font>就是一种实现了该标准的语言，同样的Adobe ActionScript也实现了ECMAScript。

因此，JavaScript就是按照 ECMAScript 规定的脚手架搭建起来的房子，所以掌握JavaScript必须要掌握ECMAScript规范。

> 最新的ECMAScript标准是2015年6月颁布的《ECMAScript2015标准》简称ES2015，由于比前面版本有了重大改进，因此又被称为ES6.0。每年6月份ES标准委员会会颁布最新的ES标准，而2016年颁布的《ECMAScript2016标准》相比较2015标准改进不大，因此又被称为ES6.1.

具体ES6标准的讲解后面博客中再讲。

## DOM 

<font color="#f07c82">DOM, Document Object Model</font>, 文档对象模型，这里指针对HTML的可编程接口。将网页通过层次性节点来表示，如下所示：

```html
<html>
    <head> 
        <title>Sample Page</title>
    </head> 
    <body>
        <p>Hello World!</p> 
    </body>
</html>
```

其实DOM可针对任何XML类语言进行实现，包括 SVG（Scalable Vector Graphic，可伸缩矢量图）1.0等。

## BOM

<font color="#f07c82">BOM, Browser Object Model</font>, 浏览器对象模型，使开发人员可以控制浏览器网页页面之外的部分。

比如：弹出新浏览器、移动缩放与关闭浏览器窗口、提供用户显示器分辨率详细信息的screen对象、对cookies支持等。

## 总结

+ <font color=#f07c82>ECMAScript</font> 标准为JavaScript提供核心语言功能；

+ <font color=#f07c82>DOM</font> 提供访问与操作网页内容的方法和接口；

+ <font color=#f07c82>BOM</font> 提供操作浏览器的方法和接口。

# JS知识点

## 在H5中使用JS

+  **引入位置：**JS的\<script\>标签放在\<body\>标签内最下方，使浏览器先加载页面再加载JS代码，保证界面流畅。

+  **嵌入外部文件：**尽量不要再 HTML 文件中嵌入 JS代码，而是引入JS外部文件的方式。提高可维护性与缓存效率。

+ **严格模式：**在整个脚本顶部或者函数内部上方加上语句<font color=#f07c82>"use strict"</font>。

    ```javascript
    function(){
        "use strict";
        //函数体，在函数内部执行严格模式
    }
    ```
## 数据类型

+ **数据类型：**JS一共6中数据类型，5中基本数据类型：<font color=#f07c82>String、Number、Boolean、Null、Undefined</font> 及一种复杂类型 <font color=#f07c82>Object</font>。基本数据类型占据固定的内存大小，存储在栈内存中。引用类型的对象保存在栈空间中。

    + JS的变量只是转递数据的载体，其本身是松散类型，一个变量即可存储字符串又可存储数组。此时需要用<font color=#f07c82>typeof</font>操作符检查数据类型。

    + 简单数据类型**变量与值**都存储在**栈内存(stack)中**。
        + 浏览器从电脑内存中开辟一块栈内存，再分配一个主线程自上而下执行程序。
        + 栈内存分为变量存储空间与值存储空间。
        + 赋值语句将两个空间对应的变量与值关联起来。
            ```javascript
            // let a = 12的赋值过程：
            // 1. 首先新建变量a(如没有)，将其存入变量存储空间
            // 2. 再新建数值12(如没有)，将其存入值存储空间。
            // 3. 再执行赋值操作 = ，将变量a与值12关联。 
                let a = 12;
                let b = a;
                b = 13;
            ```
    + 引用数据类型的变量存储在栈内存中，**值存储在堆内存(heap)**中。
        + 浏览器从电脑内存中开辟一块栈内存，再分配一个主线程自上而下执行程序。
        + 由于引用数据类型的值非常复杂，因此开辟一块堆内存，每块内存对应一个唯一的内存地址。
        + 赋值操作 = 将变量与堆内存的内存地址关联起来。
         ```javascript
                let c = {
                            name: "syz",
                            age: 24
                        };
                let d = c;
                d.age = 256;
            ```

### **基本数据类型**

+ **String类型**

    + 不同于Java,它是简单类型，但相同点是**它是不可变的**。
    + 任何数据都可转为String类型。通过调用对象的toString方法或者对于Null及Undefined直接返回"null"与"undefined"。
    + 通过 a.toString()转换。
    + 通过String(a, n)转换。n为可选项，当a为数值类型时，n可以指定转换的进制数。

+ **Number类型**

    + 分为浮点值和整型值。浮点值小数点后面必须有数字，否则认为值整型。
    + 浮点值最高精度是17位，因此不要测试某个具体的浮点值，例如<font color=#f07c82>0.3==0.1+0.2<font>。
    + Number最大值为**Number.MAX_VALUE**，最小值是**Number.MIN_VALUE**，超出范围则根据正负情况得到**正负Infinity**。
    + 用isFinite()函数判断一个数是不是无穷。
    + 0 / 0 的结果是**NaN**。它不与任何数相等，包括自身。 可用isNaN()判断一个数据是不是数。
    + parseInt(a, n) 与 parseFloat()可以解析数值。如果字符串前面有数值的话也能被解析出来。

+ **Boolean类型**

    + 可用Boolean()来对任意数值进行转换。
    + 非零及非NaN的数值(包括无限大)为真。
    + 非空的字符串为真。
    + 任何非null的对象为真。
    + undefined 始终为假。

+ **Null与Undefined**

    + null表示空指针，即声明了对象指针，但没实例化。
    + undefined表示没有初始化。
    + undefined 派生自 null，所以null==undefined,返回true。

+ 所有的类型复制都是**值传递**。

    + 对于基本类型的复制结果的修改不影响原数据，因为再拷贝了一份。
    + 复杂类型的复制结果是对**其指针的复制**，复制结果的修改会影响原数据。
    + 方法内部传递的复杂类型也是**复制后的指针**。

+ 所有**基本数据类型都是不可变的，**即使对相同名字的变量赋值，也是先销毁原先变量再生成新变量。

+ **==与===**

    + **== 是相等，即判断两者不同时，先进行类型转换。**
    + true转为数值 1， false转为0。
    + 字符串与数值比较，先转为数值。
    + 对象比较，先调用valueOf()方法转为字符串，再按照上面规则判定。
    + **NaN与任何数据比较都是false。**
    + **对象的比较是他们是否指向同一个对象。**
    + **null==undefined**返回ture。

    + **=== 是全等。如果比较的两者类型不同，则直接返回false。**

+ **方法的形参：**命名只是为了操作方便，内部实现不管形参定义，而是利用数组依次存储数据。

    + 可调用内部数组arguments读取传入的参数。
    + JS的方法不存在重载。
    ```javascript
        function add(){
            if(arguments.length==1)
                return argument[0];
            if(arguments.length==2)
                return arguments[0] + argument[1];
        }

        add(1) //输出1
        add(1,2) //输出3
    ```

+ **执行环境与作用域链**

    + **执行环境：ECMAScript代码运行的环境**
        + 执行环境用一个与之关联的**变量对象**表示，**环境中所有的变量与函数都保存在该对象内部**。环境对象的生命周期也就是内部变量与函数的声明周期。
        + 全局执行环境是最外围的执行环境，根据宿主环境不同，变量对象也不同。例如Web浏览器中的全局执行环境的变量对象为<font color=#f07c82>windows</font>，浏览器关闭就相当于 windows 变量对象死亡，内部代码也消亡。
        + 每个**函数**也有自己的**执行环境**，进入函数内，函数环境推入**环境栈**，运行完毕弹出环境栈，类似于Java的虚拟机栈，会存在栈溢出错误。

    + **作用域链：定义了可访问变量的顺序。**
        + 全局执行环境变量在最外围。
        + 函数内部变量在最前端，例如arguments。
        + 由前端向根部查找，**找到便不再往后查找，哪怕存在重名变量**。

+ **没有块级作用域**
    + 与Java等C类语言不同， if或for等块级作用域声明的变量会被加入到全局执行环境中作为全局变量。
        ```javascript
            for(var i=0; i<10; i++) {
                doSomeThing(i);
            }
            alert(i); //输出10
        ```

###  **引用数据类型**

**对象其实是某个特定引用类型的实例**，由一组键值对组成 <font color=#f07c82>`{[key]:[value]}`</font>，属性名：属性值。

+ **Onject类型**

    + 所有JS的对象类型都基于Object类，因此含有它一切的方法。
    + hasOwnProperty("propertyName"): 检查**对象实例中**属性是否存在。
    + isPrototypeOf(Object): 检查传入对象是否是传入对象的原型。
    + toLocalString(): 返回本地环境的对象字符串表示。
    + toString(): 返回对象的字符串表示。
    + valueOf(): 返回对象的数值、字符串即布尔表示，一般与toString()返回相同。

    + **创建方法**有两种。
        - 通过构造函数创建。
            ```javascript
            // 用Object构造函数创建对象
            var obj = new Object();
            // 定义属性
            obj.name = "syz";
            ```
        - 通过对象字面量表示法
            ```javascript
            // 通过花括号将要赋值内容用的键值对表示
            // 空花括号与new Object()作用相当； var obj = {};
            // 属性名可以是字符串、数字
            var obj = {
                name : "syz",
                "age": 12,
                12: 0
            }
            ```
    + **对象属性访问方式**也有两种
        - **通过对象加 `.`方式引用**，但不能引用数字及会导致引用语法错误的字符。
            ```javascript
            var person = {
                name: "syz",
                age: 12,
                12: 0
            }
            // 访问name及agr属性
            alert(person.name);
            alert(person.age);
            // 但不能访问属性 12， person.12是语法错误
            ```
        - **通过中括号+属性字符串方式引用**，可通过变量访问属性。
            ```javascript
            var person = {
                name: "syz",
                age: 12,
                12: 0
            }
            // 访问name属性
            alert(person["name"]);
            // 访问age属性
            var propertyName = "age";
            alert(person[propertyName]);
            //访问12属性
            alert(person["12"]);
            ```
+ **Array类型**
JS的数组比Java的强大太多，不仅可以支持类似于Python的切片功能，而且还可以模拟队列、栈等数据结构，还能够很方便的实现数组的迭代、拼接、插入、删除等功能。
    + **新建Array**有两种方式。
        + 通过Array构造函数创建，可**省略new关键字**。
            ```javascript
            var arr0 = new Array(); // 构建数组
            var arr1 = Array(5); // 预定义长度为5的数组，每一项为 undefined
            var arr2 = Array("1",2,"test"); // 可以存储任何类型的数据，且长度是动态扩展的
            ```
        + 通过数组字面量表示法
            ```javascript
                var arr = [1, "test"];
            ```
    + 数组的**length属性**
        数组的length属性不是可读的，可以控制它实现数据的删除与扩展操作。
        ```javascript
            var arr = [1,2,3];
            arr.length = 2;
            alert(arr[2]); // 返回unfefined
            arr.length = 99; // 2-98位置都为undefined
        ```
    + **检查数组**
        - 由于全局执行环境可能有多种（框架多种），不同环境的Array定义不同，所以采用 <font color=#f07c82>instanceOf Array</font>可能有错误。
        - 采用<font color=#f07c82>Array.isArray()</font>方法来判断。

    + **打印数组**
        - 调用toString()打印（默认采用）;
        - 采用join("||")打印，里面接收分隔符。
            ```javascript
            var a = [1,2,3];
            alert(a.join("||")); //打印 1||2||3
        ```
    + 数组的**类栈**操作
        - push()方法入栈，返回数组长度
        - pop()方法出栈，移除并返回数组最后一个元素
    
    + 数组的**类队列**操作
        - push()方法入队列
        - shift()方法出队列，即移除并返回数组第一个元素
        - unshift()方法反向入队列，即在数据第一个位置插入，并返回数组长度
        - pop()方法反向出队列，移除并返回数组最后一个元素
    
    + 数组**拼接、切片、插入与删除**
        - **concat()函数**实现拼接新元素，并返回新数组
        ```javascript
        var arr = [1,2,3];
        var arr2 = arr.concat("3",["string", 4, 5]);
        console.log(arr2); //打印 1,2,3,3,string,4,5
        ```
        -**slice(startIndex, endIndex)函数**实现数组切片,返回新数组
        ```javascript
        var arr = [1,2,3,4,5];
        var arr2 = arr.splice(1,3); 
        console.log(arr2); // 输出[2,3]

        arr2 = arr.splice(3);
        console.log(arr2); // 输出[4,5],默认从位置3开始切片到末尾

        arr2 = arr.slice(-1);
        console.log(arr2); // 输出[5],从最后的位置开始切片

        arr2 = arr.slice(-3,-1);
        console.log(arr2); // 输出[3, 4],从倒数第3位置开始切分，不包括倒数第一位置的数
        ```
        - **splice(startIndex, removeNum, insertElement)方法**插入与删除元素，返回删除的数据
        ```javascript
         var arr = [1,2,3,4,5];
         arr.splice(0, 0, [1,2,3]);
         console.log(arr); //输出[ [ 1, 2, 3 ], 1, 2, 3, 4, 5 ]， 即在首位插入[1,2,3]

         var arr = [1,2,3,4,5];
         var deleteEle = arr.splice(1, 4);
         console.log(arr); // 输出 [1]，即从第二个位置删除4项
         console.log(deleteEle);  // 输出[2,3,4,5]，即删除的数据

         var arr = [1,2,3,4,5];
         arr.splice(1,2,"insertEle");
         console.log(arr); // 输出[ 1,'insertEle', 4, 5 ]，即从第位置1删除两项并在位置1开始插入"insertEle"
         ```
    + **数组的排序**
        - **reverse()函数**，颠倒数组
        ```javascript
        var a = [1,2,3];
        console.log(a.reverse()); //输出[3,2,1]
        ```
        - **sort()函数**，排序数组，默认是先把元素转为字符串，然后排序
        ```javascript
        // 默认字符串排序
        var a = [1,5, 10, 15, 20];
        console.log(a.sort()); //输出[ 1, 10, 15, 20, 5 ]

        //传入比较函数，实现自定义排序
        function compare(v1, v2){
            // 逆序
            return v2-v1;
        }
        console.log(a.sort(compare)); //输出[[ 20, 15, 10, 5, 1 ]]
        ```
    + **数组元素位置查询**
        - indexOf(searchLocation)，从指定位置顺序查询
        - lastIndexOf(searchLocation)，从指定位置逆序查询
    + **数组迭代**
    每个迭代方法都接收两个参数：**要在每一项上运行的函数和（可选的）运行该函数的作用域对象**——影响 this 的值。传入这些方法中的函数会接收三个参数：**数组项的值、该项在数组中的位置和数组对象本身**。
        - **every()**：对数组中的每一项运行给定函数，如果该函数对**每一项**都返回 true，则返回 true。 
        - **filter()**：对数组中的每一项运行给定函数，**返回该函数会返回 true 的项组成的数组**。 
        - **forEach()**：对数组中的每一项运行给定函数。这个方法没有返回值。 
        - **map()**：对数组中的每一项运行给定函数，返回**每次函数调用的结果组成的数组**。 
        - **some()**：对数组中的每一项运行给定函数，如果该函数对**任一项**返回 true，则返回 true。
        ```javascript
        // every
        var numbers = [1,2,3,4,5,4,3,2,1];
        var everyResult = numbers.every(function(item, index, array){ 
            return (item > 2);
        }); 
        alert(everyResult); // false

        // some
        var someResult = numbers.some(function(item, index, array){ 
            return (item > 2);
        }); 
        alert(someResult); //true

        // filter
        var numbers = [1,2,3,4,5,4,3,2,1];
        var filterResult = numbers.filter(function(item, index, array){ 
            return (item > 2);
        }); 
        alert(filterResult); //[3,4,5,4,3]

        // map
        var numbers = [1,2,3,4,5,4,3,2,1];
        var mapResult = numbers.map(function(item, index, array){ 
            return item * 2;
        }); 
        alert(mapResult); //[2,4,6,8,10,8,6,4,2]
        ```
    + **数组归并**
    两个归并数组的方法：**reduce()和 reduceRight()**。归并元素生成一个值。
    两个方法都接收两个参数：一个在每一项上调用的函数和（可选的）作为归并基础的初始值。
    传给 reduce()和 reduceRight()的函数接收4个参数：**前一个值、当前值、项的索引和数组对象。**
    ```javascript
        var values = [1,2,3,4,5];
        var sum = values.reduce(function(prev, cur, index, array){ 
            return prev + cur;
        }); 
        alert(sum); //15
    ```
+ **Function类型**
所有的函数都是Function类型的实例，因此函数名也就是实例的指针，函数可以拥有方法和属性，函数也可以作为参数传入另一个函数或对象中，就跟一个对象没区别。
    + **创建函数**
        ```javascript
        /* 使用函数声明定义函数,此时存在函数声明提升的过程。即在代码还没运行前，
        将函数声明的函数解析并放到代码树的顶端，以供后面的代码使用。*/
        alert(sum1(1,2)); //可以执行，因为sum1已经被提升到最前端
        // 函数声明方式创建
        function sum1(d1, d2){
            return d1 + d2;
        }

        // 函数表达式创建
        // alert(sum(1, 2)) 报错，因为函数表达式不会有函数提升过程
        var sum = function(d1, d2){
            return d1 + d2;
        }; //有分号

        // 函数可以作为 对象/参数 传入函数
        function apply(fc, num1, num2){
            return fc(num1, num2);
        }

        function sum(d1, d2){
            return d1 + d2;
        }

        alert(apply(sum,1,2)); // 输出3
        ```
    - **apply()与call()方法**
    **两个都是Function除Object对象自带函数外内置的另外两个函数，作用是在指定的环境中运行某函数**，两个方法接收两个参数，第一个表示当前环境的指针，另一个表示传入的参数。区别是，apply第二个可以是参数属性arguments或者是数组，而call()必须是参数依次列出。
    ```javascript
    // apply应用场景
    function sum(num1, num2){ 
        return num1 + num2;
    }
    function callSum1(num1, num2){ 
        return sum.apply(this, arguments);
    }
    function callSum2(num1, num2){ 
        return sum.apply(this, [num1, num2]);
    }
    alert(callSum1(10,10)); 
    alert(callSum2(10,10));

    // call应用场景
    function sum(num1, num2){ 
        return num1 + num2;
    }
    function callSum(num1, num2){ 
        return sum.call(this, num1, num2);
    } 
    alert(callSum(10,10));

    // apply在指定环境运行函数
    var color = "red";
    var o = {color:"blue"};
    function sayColor(){
        return this.color;
    }
    alert(sayColor()); //red
    alert(sayColor.call(this)); // red 全局执行环境，即windows
    alert(sayColor.call(whidows)); // red 全局执行环境
    alert(sayColor.call(o)); // blue 执行环境为对象o
    ```
- **arguments对象与this对象**
    + arguments表示传入的参数数组，该对象具有属性length及callee。
        - lenght属性查看传入参数的数量。
        - **callee，获得包含arguments的函数指针**
        ```javascript
        // 阶乘函数
        function factorial(num){
            if(num<=1) return num;
            // 与函数名解耦
            return arguments.callee(num-1)*num;
        }

        var fc2 = factorial;
        alert(fc2(10));
        ```
    + **this对象指向表示函数的执行环境的对象。**

- **length与prototype属性**
    + length表示函数**希望**接收的参数个数。
    + prototype保存函数的所有实例方法。

+ **基本包装类型**

**JS在生成一个基本类型数据时，同时产生一个基本包装类型，为读取基本数据的一些操作提供执行方法的入口。**
```javascript
var str = "Hello JS"；
str.substring(0,5)；
str.name = "str";
alert(str.name); //输出undefined，因为执行完毕后立即销毁该包装类型对象
```
上面的例子生成一个string类型的基本数据，但是调用substring()方法时，是调用的同时生成的基本包装类型String对象内的方法。在执行完毕后该对象立马销毁。

基本包装类型有Boolean、Number和String，都有Object内置的toString()、toLocalString()以及valueOf()方法。
- **String包装类型内置方法**
    + 三种切片方法substring、slice及substr，返回新的字符串
        - **substring(startIndex, endIndex):**在索引都是正数时，与slice()一样，但是遇到负数取零。
        - **slice(startIndex, endIndex):**与数组切片方法一致。
        - **substr(startIndex, cut_length):** 第一个切片起点，第二个切片长度，长度为负数时返回空字符串""。
    + **charAt()与charCodeAt():** 分别返回所在位置的字符及字符编码。
    + **length:** 放回字符数量。
    + **concat():** 拼接字符串，返回新字符串。
    + **split():** 用指定字符切割字符串，第一个参数是切割字符，第二个可选参数为返回数组的长度。
    + **match()与search():** 子字符串的查找。
- **Number包装类型**
    + toFixed(n): 精确到小数点后n位。
    + toExponential(n): 用指数表示法表示，n为精确的小数点位数。
    + toPrecision(n): n为显示数值的位数，根据情况调用toFixed()或toExponential()方法。
- ~~Boolean包装类型~~
不推荐使用。

+ **Global对象与Math对象**

全局对象Global是JS最终的“兜底”对象，所有对象包含在其内部，不属于其它方法(如isNaN()、isFinite())与属性(如NaN、undifined)都属于它的方法与属性。例如windows对象就是一个全局对象。

<center>{% asset_img fun_global.png %}</center>

<center>**Global对象的属性**</center>

Math对象是为数学运算设计的，包括常用的max()、min()、ceil()、floor()、round()及random()方法等。其中，max()与min()与java不同的是，可以比较获得所有数据的最大值与最小值。

# 总结

1. JavaScript是按照ECMAScript语言标准设计，可操作DOM、BOM接口与方法的一门前端语言。

2. JavaScript数据类型分为五种基本数据类型与引用数据类型。

3. 基本数据类型存储在栈内存中，且是final的，即不可修改。

4. 引用数据类型的变量名存储在栈内存中存储实例的指针，内容存储在堆内存中，以键值对方式保存。

5. JavaScript语言的类型都是松散型的，且为顺序执行。

**<font color=#f07c82>让我们拥抱JS，一起迎接大前端时代吧！</font>**