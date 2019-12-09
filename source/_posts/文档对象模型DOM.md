---
title: 文档对象模型DOM
comments: true
toc: true
date: 2019-11-22 22:41:19
categories: Web前端
tags: JavaScript
---

# DOM是什么？

DOM，即Document Object Model，文档对象模型，用于操作HTML的编程接口。

它描绘了一个层次化的节点树，允许编程人员进行节点的添加、修改与删除等操作。

# DOM节点

DOM 1级定义了一个**Node**接口，由DOM中所有的节点实现。

一共有**12**中节点类型，其中包括<font color=#f07c82>Document、Element</font>节点类型等，可通过node.**nodeType**查询每个节点的类型，例如，Element的节点类型返回数值为1。

每一个节点都有**childNodes、parentNode、previousSibling、nextSibling、firstChild、lastChild**等节点查询方法。

<center>{% asset_img node.png%}</center>

还有**appendChild、insertBefore、replaceChild、removeChild、cloneNode**等操作节点的方法。

## Document

Document节点表示整个文档。浏览器中的document是一个全局对象，表示HTML页面，是HTMLDocument对象的实例。

> document.documentElement：快速取得对html的引用。
> document对象只有一个子节点，也就是< html >对象，没必要使用节点操作方法。
> document的title、URL、domain、referrer等方法可以访问网页的标题、URL地址、域名及连接当前URL的前一URL。
> document.getElementById()：获取指定id的元素。
> ducument.getElementsByTagName()：获取指定标签名的元素，例如`var img = ducument.getElementsByTagName("img")`，返回一个HTMLCollection类型的实例，其中`img.namedItem("imgName")`可以返回执行name的元素。
> ducument.getElementsByName()：获取指定name的元素集合。
> ducument.write()与ducument.writeln()写入文档。
> getElementsByClassName()：获取同一类的元素列表，返回NodeList。
> document.activeElement()：获得当前焦点所在的元素，例如页面加载中，焦点所在的元素是document.body，之后变成null。
> document.hasFocus()：判断当前文档是否获取了焦点。
> document.readyState："loading"正在加载文档，"complete"加载完毕。
> document.compatMode："CSS1Compat"渲染模式为标准，"BackCompat"渲染模式为混杂模式。
> document.charset：查看与设置文档字符集，默认"UTF-16"。

## Element

Element类型表现为HTML元素，每个HMTML元素都存在**id、title、dir、className、lang**属性。

> getAttribute() setAttribute()和 removeAttribute()操作属性值。

```javascript
var div = document.getElementById("myDiv"); 
alert(div.getAttribute("id")); //"myDiv"

div.setAttribute("id", "someOtherId");

div.removeAttribute("class");
```

> attributes属性，是表示一个元素属性的动态的集合，可通过**nodeValue**获取标签值。

```javascript
var id = div.attributes.getNamedItem("id").nodeValue;
```

>  document.createElement()创建元素。
```javascript
var div = document.createElement("div");
```

## Text类型

Text，文本节点，用于表示不含HTML标签的纯文本。包含在element类型中。

 appendData(text)：将 text 添加到节点的末尾。 
 deleteData(offset, count)：从 offset 指定的位置开始删除 count 个字符。 
 insertData(offset, text)：在 offset 指定的位置插入 text。 
 replaceData(offset, count, text)：用 text 替换从 offset 指定的位置开始到 offset+ count 为止处的文本。
 splitText(offset)：从 offset 指定的位置将当前文本节点分成两个文本节点。 
 substringData(offset, count)：提取从 offset 指定的位置开始到 offset+count 为止处的字符串。

```javascript
<!-- 没有内容，也就没有文本节点 --> 
<div></div>
<!-- 有空格，因而有一个文本节点 --> 
<div> </div>
<!-- 有内容，因而有一个文本节点 --> 
<div>Hello World!</div>

// 取得div的文本节点，并修改其值
div.firstChild.nodeValue = "Some other message";
```

> document.createTextNode()创建新文本节点。

```javascript
var element = document.createElement("div"); 
element.className = "message";
var textNode = document.createTextNode("Hello world!"); 
element.appendChild(textNode);
document.body.appendChild(element);
```

# DOM操作技术

## 动态脚本

可以通过<font color=#f07c82>doucument.createElement()</font>方法实现动态script代码的引入或创建。
```javascript
var script = document.createElement("javascript");
script.type = "text/javascript";
script.src = "client.js";
document.body.appendChild(script);
```

或者可以通过自定义代码的方式调用

```javascript
function loadScriptString(code){
    var script = document.createElement("script"); script.type = "text javascript"; 
    try {
        script.appendChild(document.createTextNode(code)); 
    } catch (ex){ 
        script.text = code; 
    } 
    document.body.appendChild(script); 
}

loadScriptString("function sayHi(){alert('hi');}");
```

## 动态样式

CSS样式可以通过<font color=#f07c82>style</font>属性或元素嵌入样式，或者可以通过< link >引入CSS外部文件。

```javascript
<link rel="stylesheet" type="text/css" href="index.css">
```

与动态script一样的原理，可以通过实现样式的动态添加。

```javascript
var link = document.createElement("link"); 
link.rel = "stylesheet"; 
link.type = "text/css"; 
link.href = "style.css";
var head = document.getElementsByTagName("head")[0]; 
head.appendChild(link);
```

或者可以通过自定义代码的方式调用

```javascript
var style = document.createElement("style"); 
style.type = "text/css"; 
try{
    style.appendChild(document.createTextNode("body{background-color:red}")); 
} catch (ex){ 
    style.styleSheet.cssText = "body{background-color:red}"; 
}
var head = document.getElementsByTagName("head")[0];
head.appendChild(style);
```

# DOM扩展

## 选择符

1. <font color=#f07c82>querySelector()与querySelectorAll()</font>方法。

> querySelector()
```javascript
//取得 body 元素
var body = document.querySelector("body"); 
//取得 ID 为"myDiv"的元素
var myDiv = document.querySelector("#myDiv"); 
//取得类为"selected"的第一个元素
var selected = document.querySelector(".selected"); 
//取得类为"button"的第一个图像元素
var img = document.body.querySelector("img.button");
```

> querySelectorAll()
```javascript
//取得某<div>中的所有<em>元素（类似于 getElementsByTagName("em")） 
var ems = document.getElementById("myDiv").querySelectorAll("em"); 
//取得类为"selected"的所有元素
var selecteds = document.querySelectorAll(".selected"); 
//取得所有<p>元素中的所有<strong>元素
var strongs = document.querySelectorAll("p strong");
```