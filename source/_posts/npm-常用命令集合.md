---
title: npm 常用命令集合
comments: true
toc: true
date: 2021-03-20 21:06:59
categories: Web前端
tags: npm
cover: https://w.wallhaven.cc/full/4y/wallhaven-4yqm8x.jpg
description: 对于 npm 的一些常用命令的汇总
---

# npm 更新命令

**查看当前 npm 版本**

```
npm -v
```

**更新到最新版本**

```
npm install -g npm
```

**更新到指定版本**

```
npm -g install npm@6.8.0
```

# npm 升级框架的依赖库

**1. 全局升级 cli 工具**

查看版本
```
hexo version
```

升级
```
npm i hexo-cli -g
```

**2. 安装 npm check 工具，检查哪些工具需要升级**

```
npm install -g npm-check
npm-check
```

**3. 升级依赖包和系统插件**

```
npm install -g npm-upgrade

npm-upgrade
```

**4. 继续升级**

```
npm update -g

npm update --save
```