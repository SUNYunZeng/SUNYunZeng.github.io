---
title: Markdown文件中添加UML图
comments: true
toc: true
date: 2019-09-04 10:23:38
categories: 教程
tags: 网页
---

# UML图简介

UML(Unified Modeling Language)是统一建模语言的简写。

它可分为用例视图、设计视图、进程视图、实现视图和拓扑视图，又可以静动分为静态视图和动态视图。静态图分为：用例图，类图，对象图，包图，构件图，部署图。动态图分为：状态图，活动图，协作图，序列图。

其中类图(Class Diagrams)是用来表示类的内部结构和类与类之间的关系的一种UML。常见的关系有：泛化(Generalization)，实现(Realization)，组合(Composition)，聚合(Aggregation)，关联(Association)，依赖(Dependency)。

各种关系的强弱顺序： 泛化 = 实现 > 组合 > 聚合 > 关联 > 依赖

<u>[参考教程](https://www.cnblogs.com/jiangds/p/6596595.html)</u>

# UML类图的绘制及插入

我们利用在线的开源工具<u>[plantUML](http://www.plantuml.com/plantuml/uml/SyfFKj2rKt3CoKnELR1Io4ZDoSa70000)</u>实现UML图绘制。

具体的绘制方法可以参考官网<u>[plantUML](http://plantuml.com/zh/)</u>。

然后将网址中生成的UML图片地址插入到**Markdown**文件中。

{% asset_img uml.png %}

<u>[参考教程](https://www.heqiangfly.com/2017/07/08development-tool-markdown-plant-uml/)</u>



