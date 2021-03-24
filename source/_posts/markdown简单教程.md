---
title: Markdown简单教程
comments: true
categories: 教程
tags: 博客
toc: true
password: ''
message: 请输入查看密码
wrong_pass_message: 抱歉，您输入的密码不对，请重试
date: 2019-04-26 10:08:24
cover:
description:
---


# Markdown语言简介

Markdown是一种轻量级的「标记语言」，语法简单易于上手，2004年由[John Gruber](https://daringfireball.net/projects/markdown/)创建。Markdown通过纯文本编写的方式实现文章的撰写与排版，然后被转化为html或者pdf，可以轻松地将文章分享。相较于文本编辑器word的文章排版的方式，Markdown通过固定的文本格式实现了绝大多数的排版场景，让创作者可以专心于文章的构思于撰写，而从繁琐的排版工作中解脱出来。

# Markdown书写工具

Markdown也得到了广泛的支持，例如简书、github、知乎、博客园等等都支持直接上传Markdown文档，同时一些桌面程序也支持利用Markdown语法记录文章。推荐的Markdown文档工具如下：
- Mac系统：[Mou](http://25.io/mou/)
- Windows系统：[MarkPad](http://code52.org/DownmarkerWPF/)
- Linux系统：[retext](https://gitee.com/mirrors/retext)
- 网页版：[简书](https://www.jianshu.com/)，可支持在线预览Markdown文档的功能

一些在线的Markdown工具：

- [MaHua](http://mahua.jser.me/?utm_source=mindstore.io)在线编辑器，无须测试 
- [Cmd Markdown](https://www.zybuluo.com/cmd/?utm_source=mindstore.io)客户端编译器

# Markdown语法

很多博客的文章也是用Markdown写成的，因此花几分钟学习Markdown的语法还是很有必要的。下面介绍最常用的几种Markdown语法。

## **标题**

```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
```

## **字体**

```
**加粗**
*斜线*
~~删除线~~
```

**加粗**
*斜线*
~~删除线~~
由于markdown支持插入html标签，因此更复杂的字体设置可以通过html标签跟css控制：
```
<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN" color=#0099ff>我是华文彩云</font>
<font color=#00ffff size=2>color=#00ffff</font>
<font color=gray size=3>color=gray</font>
<table><tr><td bgcolor=orange>背景色是：orange</td></tr></table>
```
<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN" color=#0099ff>我是华文彩云</font>
<font color=#00ffff size=2>color=#00ffff</font>
<font color=gray size=3>color=gray</font>
<table><tr><td bgcolor=orange>背景色是：orange</td></tr></table>
## **分割线**
```
*** 或者 ---
```
下面是分割线：
---

## **引用**
```
> 有一分热，发一分光，就令萤火一般，也可以在黑暗里发一点光，不必等候炬火。
> 此后如竟没有炬火，我便是唯一的光。倘若有了炬火，出了太阳，我们自然心悦诚服的消失。
> 不但毫无不平，而且还要随喜赞美这炬火或太阳；因为他照了人类，连我都在内。
```
> 有一分热，发一分光，就令萤火一般，也可以在黑暗里发一点光，不必等候炬火。此后如竟没有炬火，我便是唯一的光。倘若有了炬火，出了太阳，我们自然心悦诚服的消失。不但毫无不平，而且还要随喜赞美这炬火或太阳；因为他照了人类，连我都在内。

## **列表**
```
1. 有序列表.1
2. 有序列表.2
- 无序列表.1
- 无序列表.2
```
1. 有序列表.1
2. 有序列表.2
- 无序列表.1
- 无序列表.2

## **代码**
```
行内代码： `行内代码`
块内代码：
```javascript
let name = 'syz'
```
```
行内代码： `行内代码`
块内代码：
```javascript
let name = 'syz'
```

## **表格**
```
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
```
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

:-------------: 表示居中，-----:表示靠右居中

## **外链及图片**

```
插入链接：
[链接名称](链接地址)
```

[链接名称](链接地址)

```
![](图片地址)
```

![图片名称](图片地址)

## **公式**

行内公式：使用两个“$”符号引用公式: `$公式$`

行间公式：使用两对“$”公式`$$`

```
$$E=mc^2$$
```

$$ E=mc^2 $$

## Butterfly 主题Markdown进阶

- **Tabs**

使用方法：
```
{% tabs Unique name, [index] %}
<!-- tab [Tab caption] [@icon] -->
Any content (support inline tags too).
<!-- endtab -->
{% endtabs %}

Unique name   : Unique name of tabs block tag without comma.
                Will be used in #id's as prefix for each tab with their index numbers.
                If there are whitespaces in name, for generate #id all whitespaces will replaced by dashes.
                Only for current url of post/page must be unique!
[index]       : Index number of active tab.
                If not specified, first tab (1) will be selected.
                If index is -1, no tab will be selected. It's will be something like spoiler.
                Optional parameter.
[Tab caption] : Caption of current tab.
                If not caption specified, unique name with tab index suffix will be used as caption of tab.
                If not caption specified, but specified icon, caption will empty.
                Optional parameter.
[@icon]       : FontAwesome icon name (full-name, look like 'fas fa-font')
                Can be specified with or without space; e.g. 'Tab caption @icon' similar to 'Tab caption@icon'.
                Optional parameter.
```

Demo 1

```
{% tabs test1 %}
<!-- tab -->
**This is Tab 1.**
<!-- endtab -->

<!-- tab -->
**This is Tab 2.**
<!-- endtab -->

<!-- tab -->
**This is Tab 3.**
<!-- endtab -->
{% endtabs %}

```

{% tabs test1 %}
<!-- tab -->
**This is Tab 1.**
<!-- endtab -->

<!-- tab -->
**This is Tab 2.**
<!-- endtab -->

<!-- tab -->
**This is Tab 3.**
<!-- endtab -->
{% endtabs %}

- **标签外挂**

```
{% note [class] [no-icon] [style] %}
Any content (support inline tags too.io).
{% endnote %}
```

| 名称        | 用法  |
| :-------------: |:-------------:| 
| class     | 【可选】标签，不同的标签有不同的配色<br>（ default / primary / success / info / warning / danger ）|
| no-icon     | 【可选】不显示 icon      | 
| style | 【可选】可以覆盖配置中的 style  <br> （simple/modern/flat/disabled）| 


```
{% note simple %}
默认 提示块标籤
{% endnote %}

{% note default simple %}
default 提示块标籤
{% endnote %}

{% note primary simple %}
primary 提示块标籤
{% endnote %}

{% note success simple %}
success 提示块标籤
{% endnote %}

{% note info simple %}
info 提示块标籤
{% endnote %}

{% note warning simple %}
warning 提示块标籤
{% endnote %}

{% note danger simple %}
danger 提示块标籤
{% endnote %}
```

{% note simple %}
默认 提示块标籤
{% endnote %}

{% note default simple %}
default 提示块标籤
{% endnote %}

{% note primary simple %}
primary 提示块标籤
{% endnote %}

{% note success simple %}
success 提示块标籤
{% endnote %}

{% note info simple %}
info 提示块标籤
{% endnote %}

{% note warning simple %}
warning 提示块标籤
{% endnote %}

{% note danger simple %}
danger 提示块标籤
{% endnote %}

```
{% note modern %}
默认 提示块标籤
{% endnote %}

{% note default modern %}
default 提示块标籤
{% endnote %}

{% note primary modern %}
primary 提示块标籤
{% endnote %}

{% note success modern %}
success 提示块标籤
{% endnote %}

{% note info modern %}
info 提示块标籤
{% endnote %}

{% note warning modern %}
warning 提示块标籤
{% endnote %}

{% note danger modern %}
danger 提示块标籤
{% endnote %}
```

{% note modern %}
默认 提示块标籤
{% endnote %}

{% note default modern %}
default 提示块标籤
{% endnote %}

{% note primary modern %}
primary 提示块标籤
{% endnote %}

{% note success modern %}
success 提示块标籤
{% endnote %}

{% note info modern %}
info 提示块标籤
{% endnote %}

{% note warning modern %}
warning 提示块标籤
{% endnote %}

{% note danger modern %}
danger 提示块标籤
{% endnote %}

```
{% note flat %}
默认 提示块标籤
{% endnote %}

{% note default flat %}
default 提示块标籤
{% endnote %}

{% note primary flat %}
primary 提示块标籤
{% endnote %}

{% note success flat %}
success 提示块标籤
{% endnote %}

{% note info flat %}
info 提示块标籤
{% endnote %}

{% note warning flat %}
warning 提示块标籤
{% endnote %}

{% note danger flat %}
danger 提示块标籤
{% endnote %}
```

{% note flat %}
默认 提示块标籤
{% endnote %}

{% note default flat %}
default 提示块标籤
{% endnote %}

{% note primary flat %}
primary 提示块标籤
{% endnote %}

{% note success flat %}
success 提示块标籤
{% endnote %}

{% note info flat %}
info 提示块标籤
{% endnote %}

{% note warning flat %}
warning 提示块标籤
{% endnote %}

{% note danger flat %}
danger 提示块标籤
{% endnote %}

```
{% note disabled %}
默认 提示块标籤
{% endnote %}

{% note default disabled %}
default 提示块标籤
{% endnote %}

{% note primary disabled %}
primary 提示块标籤
{% endnote %}

{% note success disabled %}
success 提示块标籤
{% endnote %}

{% note info disabled %}
info 提示块标籤
{% endnote %}

{% note warning disabled %}
warning 提示块标籤
{% endnote %}

{% note danger disabled %}
danger 提示块标籤
{% endnote %}
```

{% note disabled %}
默认 提示块标籤
{% endnote %}

{% note default disabled %}
default 提示块标籤
{% endnote %}

{% note primary disabled %}
primary 提示块标籤
{% endnote %}

{% note success disabled %}
success 提示块标籤
{% endnote %}

{% note info disabled %}
info 提示块标籤
{% endnote %}

{% note warning disabled %}
warning 提示块标籤
{% endnote %}

{% note danger disabled %}
danger 提示块标籤
{% endnote %}

