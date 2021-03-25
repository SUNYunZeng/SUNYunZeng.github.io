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
katex: true
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

```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
```

## **字体**

```markdown
**加粗**
*斜线*
~~删除线~~
```

**加粗**
*斜线*
~~删除线~~
由于markdown支持插入html标签，因此更复杂的字体设置可以通过html标签跟css控制：
```markdown
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
```markdown
*** 或者 ---
```
下面是分割线：

---

## **引用**
```markdown
> 有一分热，发一分光，就令萤火一般，也可以在黑暗里发一点光，不必等候炬火。
> 此后如竟没有炬火，我便是唯一的光。倘若有了炬火，出了太阳，我们自然心悦诚服的消失。
> 不但毫无不平，而且还要随喜赞美这炬火或太阳；因为他照了人类，连我都在内。
```
> 有一分热，发一分光，就令萤火一般，也可以在黑暗里发一点光，不必等候炬火。此后如竟没有炬火，我便是唯一的光。倘若有了炬火，出了太阳，我们自然心悦诚服的消失。不但毫无不平，而且还要随喜赞美这炬火或太阳；因为他照了人类，连我都在内。

## **列表**
```markdown
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
```markdown
行内代码： `行内代码`
块内代码：
```javascript
let name = 'syz'```
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
插入图片：
![](图片地址)
```

![图片名称](图片地址)

## **公式**

行内公式：使用两个“ $ ”符号引用公式: `$公式$`

行间公式：使用两对“ $ ”公式`$$`

本博客采用的数学公式库为 [katex](https://katex.org/)，它是一个支持HTML的轻量级的数学公式引擎，使用起来也非常简单。


可以通过[在线可视化公式编辑器](https://demo.wiris.com/mathtype/en/developers.php)，实现公式代码的生成

```
行内公式：$E=mc^2$
```

行内公式：$E=mc^2$

```
行间公式：
$$\begin{bmatrix}1&2\\1&4\end{bmatrix}=\int_0^1\lim_{x\rightarrow\infty}\operatorname dx$$
```

$$\begin{bmatrix}1&2\\1&4\end{bmatrix}=\int_0^1\lim_{x\rightarrow\infty}\operatorname dx$$


## Markdown进阶

### **Tabs**

使用方法：
```markdown
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

> Demo 1 - 预设选择第一个【默认】

```markdown
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

> Demo 2 - 预设选择tabs

```markdown
{% tabs test2, 3 %}
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

{% tabs test2, 3 %}
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

> Demo 3 - 没有预设值

```markdown
{% tabs test3, -1 %}
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

{% tabs test3, -1 %}
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

> Demo 4 - 自定义Tab名 + 只有icon + icon和Tab名

```markdown
{% tabs test4 %}
<!-- tab 第一个Tab -->
**tab名字为第一个Tab**
<!-- endtab -->

<!-- tab @fab fa-apple-pay -->
**只有图标 没有Tab名字**
<!-- endtab -->

<!-- tab 炸弹@fas fa-bomb -->
**名字+icon**
<!-- endtab -->
{% endtabs %}
```

{% tabs test4 %}
<!-- tab 第一个Tab -->
**tab名字为第一个Tab**
<!-- endtab -->

<!-- tab @fab fa-apple-pay -->
**只有图标 没有Tab名字**
<!-- endtab -->

<!-- tab 炸弹@fas fa-bomb -->
**名字+icon**
<!-- endtab -->
{% endtabs %}

### **Note便签**

{% tabs 通用设置 %}
<!-- tab 通用设置 -->
移植于next主题，并进行修改。

修改 `主题配置文件`
```yaml
note:
  # Note tag style values:
  #  - simple    bs-callout old alert style. Default.
  #  - modern    bs-callout new (v2-v3) alert style.
  #  - flat      flat callout style with background, like on Mozilla or StackOverflow.
  #  - disabled  disable all CSS styles import of note tag.
  style: simple
  icons: false
  border_radius: 3
  # Offset lighter of background in % for modern and flat styles (modern: -12 | 12; flat: -18 | 6).
  # Offset also applied to label tag variables. This option can work with disabled note tag.
  light_bg_offset: 0
```
`icons`和`light_bg_offset`只对方法一生效

Note 标签外挂有两种用法
<!-- endtab -->

<!-- tab 用法1 -->
```markdown
{% note [class] [no-icon] [style] %}
Any content (support inline tags too.io).
{% endnote %}
```

| 名称        | 用法  |
| :-------------: |:-------------:| 
| class     | 【可选】标签，不同的标签有不同的配色<br>（ default / primary / success / info / warning / danger ）|
| no-icon     | 【可选】不显示 icon      | 
| style | 【可选】可以覆盖配置中的 style  <br> （simple/modern/flat/disabled）| 

> simple
```markdown
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

> modern
```markdown
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

> flat
```markdown
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

> disabled
```markdown
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

<!-- endtab -->

<!-- tab 用法2（自定义icon） -->
```
{% note [color] [icon] [style] %}
Any content (support inline tags too.io).
{% endnote %}
```

| 名称        | 用法  |
| :-------------: |:-------------:| 
| color     | 【可选】顔色 <br>(default / blue / pink / red / purple / orange / green)|
| icon     | 【可选】可配置自定义 icon (只支持 fontawesome 图标, 也可以配置 no-icon )     | 
| style | 【可选】可以覆盖配置中的 style  <br> （simple/modern/flat/disabled）| 

> simple
```markdown
{% note 'fab fa-cc-visa' simple %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' simple %}
2021年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' simple %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' simple%}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' simple %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' simple %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' simple %}
前端最讨厌的浏览器
{% endnote %}
```
{% note 'fab fa-cc-visa' simple %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' simple %}
2021年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' simple %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' simple%}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' simple %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' simple %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' simple %}
前端最讨厌的浏览器
{% endnote %}

> modern
```markdown
{% note 'fab fa-cc-visa' modern %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' modern %}
2021年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' modern %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' modern%}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' modern %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' modern %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' modern %}
前端最讨厌的浏览器
{% endnote %}
```
{% note 'fab fa-cc-visa' modern %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' modern %}
2021年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' modern %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' modern%}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' modern %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' modern %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' modern %}
前端最讨厌的浏览器
{% endnote %}

> flat
```markdown
{% note 'fab fa-cc-visa' flat %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' flat %}
2021年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' flat %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' flat%}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' flat %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' flat %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' flat %}
前端最讨厌的浏览器
{% endnote %}
```
{% note 'fab fa-cc-visa' flat %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' flat %}
2021年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' flat %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' flat%}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' flat %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' flat %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' flat %}
前端最讨厌的浏览器
{% endnote %}

> disabled
```markdown
{% note 'fab fa-cc-visa' disabled %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' disabled %}
2021年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' disabled %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' disabled %}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' disabled %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' disabled %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' disabled %}
前端最讨厌的浏览器
{% endnote %}
```

{% note 'fab fa-cc-visa' disabled %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' disabled %}
2021年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' disabled %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' disabled %}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' disabled %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' disabled %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' disabled %}
前端最讨厌的浏览器
{% endnote %}

> no-icon
```markdown
{% note no-icon %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue no-icon %}
2021年快到了....
{% endnote %}
{% note pink no-icon %}
小心开车 安全至上
{% endnote %}
{% note red no-icon %}
这是三片呢？还是四片？
{% endnote %}
{% note orange no-icon %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple no-icon %}
剪刀石头布
{% endnote %}
{% note green no-icon %}
前端最讨厌的浏览器
{% endnote %}
```
{% note no-icon %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue no-icon %}
2021年快到了....
{% endnote %}
{% note pink no-icon %}
小心开车 安全至上
{% endnote %}
{% note red no-icon %}
这是三片呢？还是四片？
{% endnote %}
{% note orange no-icon %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple no-icon %}
剪刀石头布
{% endnote %}
{% note green no-icon %}
前端最讨厌的浏览器
{% endnote %}
<!-- endtab -->
{% endtabs %}

### tag-hide

{% note warning flat %}
请注意，tag-hide内的标签外挂content内都不建议有h1 - h6 等标题。因为Toc会把隐藏内容标题也显示出来，而且当滚动屏幕时，如果隐藏内容没有显示出来，会导致Toc的滚动出现异常。
{% endnote %}

{% tabs name %}
<!-- tab Inline -->
`inline` 在文本里面添加按钮隐藏内容，只限文字
( content不能包含英文逗号，可用`&sbquo`;)
```markdown
{% hideInline content,display,bg,color %}
```
- content: 文本内容
- display: 按钮显示的文字(可选)
- bg: 按钮的背景颜色(可选)
- color: 按钮文字的颜色(可选)

> Demo 
```markdown
哪个英文字母最酷？ {% hideInline 因为西装裤(C装酷),查看答案,#FF7242,#fff %}

门里站着一个人? {% hideInline 闪 %}
```
哪个英文字母最酷？ {% hideInline 因为西装裤(C装酷),查看答案,#FF7242,#fff %}

门里站着一个人? {% hideInline 闪 %}
<!-- endtab -->


<!-- tab Block -->
`block`独立的block隐藏内容，可以隐藏很多内容，包括图片，代码块等等

( display 不能包含英文逗号，可用`&sbquo`;)

```
{% hideBlock display,bg,color %}
content
{% endhideBlock %}
```

- content: 文本内容
- display: 按钮显示的文字(可选)
- bg: 按钮的背景颜色(可选)
- color: 按钮文字的颜色(可选)

> Demo 

```markdown
查看答案
{% hideBlock 查看答案 %}
傻子，怎么可能有答案
{% endhideBlock %}
```

查看答案
{% hideBlock 查看答案 %}
傻子，怎么可能有答案
{% endhideBlock %}
<!-- endtab -->


<!-- tab Toggle -->
如果你需要展示的内容太多，可以把它隐藏在收缩框里，需要时再把它展开。

( display 不能包含英文逗号，可用`&sbquo`;)
```
{% hideToggle display,bg,color %}
content
{% endhideToggle %}
```
> Demo
```markdown
{% hideToggle Butterfly安装方法 %}
在你的博客根目录里

git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly

如果想要安装比较新的dev分支，可以

git clone -b dev https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly

{% endhideToggle %}
```

{% hideToggle Butterfly安装方法 %}
在你的博客根目录里

git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly

如果想要安装比较新的dev分支，可以

git clone -b dev https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly

{% endhideToggle %}

<!-- endtab -->
{% endtabs %}

### mermaid 绘图

{% note warning flat %}
mermaid标签不允许嵌套于一些隐藏属性的标签外挂，例如: tag-hide内的标签外挂和tabs标签外挂，这会导致mermaid图示无法正常显示，使用时请留意。

请不要压缩html代码，不然会导致mermaid显示异常
{% endnote %}

使用mermaid标签可以绘製Flowchart（流程图）、Sequence diagram（时序图 ）、Class Diagram（类别图）、State Diagram（状态图）、Gantt（甘特图）和Pie Chart（圆形图），具体可以查看[mermaid文档](https://mermaid-js.github.io/mermaid/#/)

修改 `主题配置文件`

```yaml
mermaid:
  enable: true
  # built-in themes: default/forest/dark/neutral
  theme: default
```

写法：

```markdown
{% mermaid %}
内容
{% endmermaid %}
```

例如：

```markdown
{% mermaid %}
pie
    title Key elements in Product X
    "Calcium" : 42.96
    "Potassium" : 50.05
    "Magnesium" : 10.01
    "Iron" :  5
{% endmermaid %}
```

![mermaid绘图](https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/hexo-theme-butterfly-docs-mermaid.png)

### Button

使用方法：

```markdown
{% btn [url],[text],[icon],[color] [style] [layout] [position] [size] %}

[url]         : 链接
[text]        : 按钮文字
[icon]        : [可选] 图标
[color]       : [可选] 按钮背景顔色(默认style时）
                      按钮字体和边框顔色(outline时)
                      default/blue/pink/red/purple/orange/green
[style]       : [可选] 按钮样式 默认实心
                      outline/留空
[layout]      : [可选] 按钮佈局 默认为line
                      block/留空
[position]    : [可选] 按钮位置 前提是设置了layout为block 默认为左边
                      center/right/留空
[size]        : [可选] 按钮大小
                      larger/留空

```

> Demo

```markdown
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,larger %}
```

This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,larger %}

```markdown
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block center larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block right outline larger %}
```

{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block center larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block right outline larger %}

**more than one button in center**

```markdown
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,blue larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,pink larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,red larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,purple larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,orange larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,green larger %}
```

{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,blue larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,pink larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,red larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,purple larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,orange larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,green larger %}


```markdown
<div class="btn-center">
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline blue larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline pink larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline red larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline purple larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline orange larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline green larger %}
</div>
```

<div class="btn-center">
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline blue larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline pink larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline red larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline purple larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline orange larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline green larger %}
</div>

### inlineimg

主题中的图片都是默认以`块级元素`显示，如果你想以`内联元素`显示，可以使用这个标签外挂。

```markdown
{% inlineImg [src] [height] %}

[src]      :    图片链接
[height]   ：   图片高度限制【可选】
```

> Demo 

```markdown
你看我长得漂亮不

![](https://i.loli.net/2021/03/19/2P6ivUGsdaEXSFI.png)

我觉得很漂亮 {% inlineImg https://i.loli.net/2021/03/19/5M4jUB3ynq7ePgw.png 150px %}
```

![图片示例](https://cdn.jsdelivr.net/gh/jerryc127/CDN@m2/img/hexo-theme-butterfly-docs-inlineimg.png)