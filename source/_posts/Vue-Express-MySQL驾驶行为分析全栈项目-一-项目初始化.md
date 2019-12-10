---
title: 'Vue + Express + MySQL驾驶行为分析全栈项目(一): 项目初始化'
comments: true
toc: true
date: 2019-12-04 15:54:38
categories: 教程
tags: Vue
---

# 背景

自己做了一点点的小尝试：基于前馈神经网络 LSTM 的个体出行目的地预测模型，基于个体历史出行数据，模型可以实现出行目的地的实时动态预测功能。

模型其实具有实际应用功能，为了对其应用场景进行探索，拟开发一个全栈的项目，在Web客户端实现用户出行的动态预测效果，同时能够提供数据可视分析等功能。

**[项目地址](https://github.com/SUNYunZeng/AIforDriving)**

**[可视化效果](http://geoai.sunyunzeng.com/)**

{% asset_img dy-pre.jpg %}
{% asset_img pre.gif %}
<center>动态预测效果</center>

## 技术栈

**1. 前端界面：**[Vue](https://cn.vuejs.org/)框架 + [iView组件库](https://www.iviewui.com/) + ES6标准 + [Webpack](https://www.webpackjs.com/)资源打包工具 + [Leftlet](https://leafletjs.com/)地图交互JS库 + [ECharts](https://www.echartsjs.com/zh/index.html)图表可视化库。

> **Vue**是一个**渐进式的前端开发框架。**它只关注图层界面(不像Angular一样提供全家桶)，通过数据的双向绑定解放开发者，让开发者可以专注于业务逻辑的代码。同时，Vue简单易上手，初学者完全不需要学习任何的新知识。基于Vue搭建前端界面的逻辑，包括界面模块层级关系、路由、操作逻辑等等。
> **iView组件库**是一个基于Vue.js的可视化组件库，提供高质量的前端组件，例如按钮、表格、任务栏等等，基于该组件库提供用户交互的入口。
> **ES6标准**是JavaScript的新一代标准，规范了类的继承，增加了模块的定义与引入、变量的解析赋值、以及 **let、const**命令语法等等。虽然很多是ES5的语法糖，但使得JS更加严谨与灵活。然而很多项目都是用ES6前版本实现的，以及很多浏览器并没完全支持ES6语法，为了向下兼容，采用[babel](https://www.babeljs.cn/)编译器对ES6代码编译。
> **Webpack**一种模块化的解决方案，通过模块化提高js代码的复用性，降低项目的耦合性，提高可维护性。利用Webpack使我们的项目模块化，并进行项目的打包。
> **Leftlet**与**ECharts**是两个优秀的前端可视化库，利用它们，项目实现地图轨迹的可视化。

**2. 服务器：**[Express](http://www.expressjs.com.cn/)框架 + [Sequelize](https://sequelize.org/)数据库查询管理库。

> **Express框架：**基于Node.js的Web开发框架，可以快速地搭建一个完整功能的网站。本项目基于Express实现了路由的管理与处理、数据库的CRUD操作、深度学习模型的python脚本调用。
> **Sequelize**是一个基于Node.js的对象关系映射（Object Relational Mapping，ORM）实现，通过对SQL语言的封装，实现model与SQL的映射，让开发者可以用OOP的方式操作数据库，优雅的生成安全、可维护的SQL代码。

**3. 数据库：** [MySQL](https://www.mysql.com/)关系型数据库。

## 本教程的食用准则

1. 需要有一定的前端基础，包括JS、H5、CSS3基础知识的掌握。

2. 对npm等包管理器有所了解。

3. 具有一定的面向对象的开发经验，具有模块化编程思想。

4. 对于数据库的SQL有初步掌握。

5. 了解Vue等前端框架。

# 项目总框架

如下图所示，项目框架是基于Vue项目框架实现，首先通过Vue-cli搭建Vue项目(此处网上教程很多，略过)，然后再框架下创建Server文件夹，用来存放服务器端代码。

<center>{%asset_img structure.png %}</center>

其中绿色标出的部分并是我自己创建的，可以忽略。

