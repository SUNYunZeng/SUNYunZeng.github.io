---
title: 在Linux系统中同步更新我们的Github博客
toc: true
date: 2019-04-28 16:45:48
categories: 教程
tags: 博客
---
# 原理介绍
类似于**版本管理**，我们把hexo博客文件系统在Github上建立一个分支，通过管理分支提交最新的博客文件系统，保证我们博客框架的更新。然后我们基于最新的**博客框架**，撰写文章，进行Github Pages网页的更新，即我们博客内容的更新。

**创建两个分支，一个用于存放博客静态资源，一个用于备份博客的部署文件。**

[参考资料](https://www.jianshu.com/p/0b1fccce74e0)

# 前期准备

>1. 一台Linux系统的电脑（我的linux系统是Ubuntu 18.04.2 LTS）
>2. 安装`node.js` 及 `npm`
>3. 安装 `ssh`
>4. 安装 `git`
>5. 按照我们前面教程搭建好的博客

# 创建Github 博客仓库分支

首先在我们搭建Hexo博客的电脑上，创建我们博客仓库在Github上的分支。

1. 打开我们的博客仓库，选择**setting**：

    {% asset_img  1.png %}

2. 选择**Branches**,创建博客仓库的Hexo分支，并将其设置为默认分支。

    {% asset_img  2.png %}

3. 将该仓库克融到本地，并把本地博客的(**所有Hexo目录下文件**)全部拷贝进username.github.io文件目录中去。
{% asset_img  4.png %}
{% asset_img  5.png %}

4. 然后把更新后的博客部署文件提交到Github的hexo分支中。


依次执行
```
git add .
git commit -m 'some info'
git push
```
>**注意事项**：
>* 将themes目录以内中的主题的.git目录删除（如果有），因为一个git仓库中不能包含另一个git仓库，提交主题文件夹会失败。

5. 然后我们的博客备份分支就创建好了。

# Linux 部署项目


1. [安装Node.js 与 npm](https://www.cnblogs.com/xiyuan2016/p/7878304.html)

2. [安装git](https://www.cnblogs.com/lxm20145215----/p/5905765.html)

3. [linux与Github建立联系](https://blog.csdn.net/angus_01/article/details/80118088)

4. 把我们创建好的博客备份仓库`clone`到本机（linux）

    4.1 在要保存的位置执行语句 `git clone git@github.com:your.github.io/your.github.io.git`

    4.2 **进入username.github.io目录，执行`npm install`** (由于仓库有一个.gitignore文件，里面默认是忽略掉 node_modules文件夹的，也就是说仓库的hexo分支并没有存储该目录，所以要安装依赖)

5. 安装hexo插件

    `sudo npm install hexo-cli -g`

6. 撰写我们的博客文章，然后依次执行下面指令，更新我们的博客备份。

    ```
    hexo clean
    hexo g
    git add .
    git commit -m 'some info'
    git push
    ```
7. 更新博客界面

    `hexo d`
    
# 注意事项

>**1. 每当撰写博客前，先 `git pull`，获取最新的博客备份。**

>**2. 写完博客后，先依次执行`hexo clean  、 hexo g 、 git add .  、 git commit -m 'some info  、 git push'`进行博客备份部署文件更新。**

>**3. 最后执行 `hexo  d`部署博客界面，实现博客更新。** 