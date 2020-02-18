---
title: CSS3 知识要点
comments: true
toc: true
date: 2020-02-18 12:21:10
categories: CSS3
tags: CSS3
---

# 选择元素

1. 基本元素选择

```css
/* 把段落文本设置成红色，12像素大，粗体 */
p {color:red; font-size:12px; font-weight:bold;}
```

2. 上下文选择

```css
/* 选择祖先元素为 article 标签的所有段落 p 后代 */
article p {color:red; font-size:12px; font-weight:bold;}
```

3. 特殊上下文选择

**子选择符**
```css
/* 将 section 的 h2 子元素的字体设置为 italic */
section > h2 {font-style:italic;}
```

**紧邻同胞选择**

```css
/* 将紧邻着 h2 的元素同胞 p 选择设置 */
h2 + p {font-style:italic;}
```

**一般同胞选择**
```css
/* 将 h2 与 p 之间的同胞全部选择 */
h2 ~ p {font-style:italic;}
```

**通用选择**

```css
/* 全文所有字体颜色（包括文本框）设置为黑色 */
* {color: black}
/* 只把 p 元素中所有元素文本设置为红色 */
p *{color: red}
```

**类选择**

```css
/* 把属于 specialtext 类的元素设置字体类型 */
.specialtext {font-style:italic;}
```

**标签带类选择**

```css
/* 把 p元素中属于 specialtext 类的元素设置字体类型 */
p.specialtext {font-style:italic;}
```

**ID选择**
```css
/*把 ID 为 mainmenu 元素中的 a 元素的字体颜色修改*/
#mainmenu a {color:orange;}
``` 

**属性名选择**

```css
/*将带有 title 属性的 HTML img 元素显示 2 像素宽的蓝色边框*/
img[title] {{border:2px solid blue;}}
```

**伪类选择**

伪类就是当 HTML 元素在某种状态时触发显示相应的 CSS 样式，伪类的用法是 元素名:状态 {属性名: 值}

例如：
```css
a:link {color:black;}
```

主要的伪类状态有以下几种：

状态名 | 描述 
:-:|:-:
link | 链接初始状态
visited | 链接被点击过状态
hover | 鼠标悬浮上去状态
active | 鼠标正在被点击的状态
focus | 表单获取焦点时状态
target | 对于设置为目标的元素被选择时的状态
first-child | 一组同胞元素的第一个元素
last-child | 一组同胞元素中最后一个元素
nth-child | 一组同胞元素中第 n 个元素

**伪元素选择**

两个冒号后面加伪元素状态，常用的有 ::first-letter (首字母状态)、::first-line (首行状态)、::before与::after (特定元素前后添加特殊内容)。

```html
<p class="age">25</p>
```
```css
p.age::before {content:"Age: ";} 
p.age::after {content:" years.";}

/* 显示如下：Age: 25 years.*/
```

# CSS样式叠加顺序

CSS 样式叠加遵循以下四个规则：

**1. 首先找到各元素所有的 CSS 声明。**

**2. 按照以下顺序层叠显示，越靠后的 CSS 定义方式显示级别越高。**

+ 浏览爱情默认样式
+ 用户自定义样式
+ 链接样式 （`<link href="style.css" rel=stylesheet type="text/css" />`）
+ 嵌入样式 
```html
<head>
<!-- 其他 head 元素（如 meta、title）放在这里 --> 
    <style type="text/css"> 
        h1 {font-size:16px;} 
        p {color:blue;}
    </style> 
</head>
```
+ 行内样式

```html
<p style="font-size: 12px; font-weight:bold; font-style:italic; color:red;">
By adding inline CSS styling to this paragraph, you override the default styles.
</p>
```

如果 css 里面的权重定义是 **!important**，则不考虑 CSS样式的优先级规则，优先使用。

**3. 按照特指度确定显示优先级**

特指度指的是说明元素信息的详细程度，例如 元素 p 的特指度就要低于某一 css 类的特指度。

计算规则：I-C-E

I-C-E 表示 ID、Class、Element三个维度的得分三位数，初始为 0-0-0。

如果样式里指明一个 ID，I 代表的数字 +1。

如果样式里指明一个 类 class，C 代表的数字 +1。

如果样式里指明一个 元素 Element，E代表的数字 +1。

例如：

···css
p                              0-0-1 特指度=1 
p.largetext                    0-1-1 特指度=11 
p#largetext                    1-0-1 特指度=101
body p#largetext               1-0-2 特指度=102
body p#largetext ul.mylist     1-1-3 特指度=113        
body p#largetext ul.mylist li  1-1-4 特指度=114
···

**4. 特指度相同的情况下按照叠加顺序**

# 盒模型

令人困惑的盒子属性。

每个元素就是一个盒子，其有对应的形状，对应的属性有三组：**边框（border）、内边距（padding）及外边距 （margin）。**

<center>{% asset_img box.jpg %}</center>

**1. 垂直方向的外边距取最大值。**

如果有两个段落 p，其CSS样式如下：

```css
p {height:50px; border:1px solid #000; backgroundcolor:#fff; margin-top:50px; margin-bottom:30px;}
```

那么它们之间的距离为 **50px，**而不是 50px + 30px= 80px。

**2. 水平方向外边距叠加。**

**3. 盒子的大小视 width 属性而定。**

+ 在 width 没有指定情况下，元素的宽度与父元素一致。

此时设置盒子的 **border** 及 **padding** 会向内拓展，不改变盒子大小。

设置 **margin** 会使元素宽度相对父元素的宽度减去两倍的外边距。

+ 在指定了 width 宽度大小情况下，元素宽度会拓展。

此时设置盒子的 **border** 及 **padding** 会向外拓展，使盒子变大。

+ 添加 **box-sizing:border-box** 属性，使盒子在指定宽度 width 同时，也能像未指定一样工作。
