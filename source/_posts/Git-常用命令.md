---
title: Git 常用命令
comments: true
toc: true
date: 2019-09-11 12:31:05
categories: Git
tags: Git
---

# Git简介

Git是世界上<font color=#f07c82>最先进的</font>**分布式**版本控制系统。

当时是为了帮助管理 **linux** 内核开发而开发的一个开源码版本控制系统。

{% asset_img git_priciple.jpg %}

+ Workspace：工作区

+ Index / Stage：暂存区
+ Repository：仓库区（或本地仓库）
+ Remote：远程仓库

Git 是分布式版本控制系统，每个人的电脑上都有完整的版本库。

# 常用命令列表

## 常用

```git
$ git remote add origin git@github.com:yeszao/dofiler.git         # 配置远程git版本库
$ git pull origin master                                          # 下载代码及快速合并 
$ git add .                       # 跟踪所有改动过的文件
$ git add <file>                  # 跟踪指定的文件
$ git commit -m “commit message”  # 提交所有更新过的文件
$ git push origin master                                          # 上传代码及快速合并
$ git fetch origin                                                # 从远程库获取代码

$ git branch                                                      # 显示所有分支
$ git checkout master                                             # 切换到master分支
$ git checkout -b dev                                             # 创建并切换到dev分支
$ git commit -m "first version"                                   # 提交

$ git status                                                      # 查看状态
$ git log                                                         # 查看提交历史

$ git config --global core.editor vim                             # 设置默认编辑器为vim（git默认用nano）
$ git config core.ignorecase false                                # 设置大小写敏感
$ git config --global user.name "YOUR NAME"                       # 设置用户名
$ git config --global user.email "YOUR EMAIL ADDRESS"             # 设置邮箱
```

## 别名Alias

```git
$ git config --global alias.br="branch"                 # 创建/查看本地分支
$ git config --global alias.co="checkout"               # 切换分支
$ git config --global alias.cb="checkout -b"            # 创建并切换到新分支
$ git config --global alias.cm="commit -m"              # 提交
$ git config --global alias.st="status"                 # 查看状态
$ git config --global alias.pullm="pull origin master"  # 拉取分支
$ git config --global alias.pushm="push origin master"  # 提交分支
$ git config --global alias.log="git log --oneline --graph --decorate --color=always" # 单行、分颜色显示记录
$ git config --global alias.logg="git log --graph --all --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(bold white)— %an%C(reset)%C(bold yellow)%d%C(reset)' --abbrev-commit --date=relative" # 复杂显示
```

## 创建版本库

```git
$ git clone <url>                 # 克隆远程版本库
$ git init                        # 初始化本地版本库
```

## 修改和提交

```git
$ git status                      # 查看状态
$ git diff                        # 查看变更内容
$ git add .                       # 跟踪所有改动过的文件
$ git add <file>                  # 跟踪指定的文件
$ git mv <old> <new>              # 文件改名
$ git rm <file>                   # 删除文件
$ git rm --cached <file>          # 停止跟踪文件但不删除
$ git commit -m “commit message”  # 提交所有更新过的文件
$ git commit --amend              # 修改最后一次提交
```

## 查看提交历史

```git
$ git log                         # 查看提交历史
$ git log -p <file>               # 查看指定文件的提交历史
$ git blame <file>                # 以列表方式查看指定文件的提交历史
```

## 撤销

```git
$ git reset --hard HEAD           # 撤消工作目录中所有未提交文件的修改内容
$ git reset --hard <version>      # 撤销到某个特定版本
$ git checkout HEAD <file>        # 撤消指定的未提交文件的修改内容
$ git checkout -- <file>          # 同上一个命令
$ git revert <commit>             # 撤消指定的提交
```

## 分支与标签

```git
$ git branch                      # 显示所有本地分支
$ git checkout <branch/tag>       # 切换到指定分支或标签
$ git branch <new-branch>         # 创建新分支
$ git branch -d <branch>          # 删除本地分支
$ git tag                         # 列出所有本地标签
$ git tag <tagname>               # 基于最新提交创建标签
$ git tag -a "v1.0" -m "一些说明"  # -a指定标签名称，-m指定标签说明
$ git tag -d <tagname>            # 删除标签

$ git checkout dev                # 合并特定的commit到dev分支上
$ git cherry-pick 62ecb3
```

## 合并与衍合

```git
$ git merge <branch>              # 合并指定分支到当前分支
$ git merge --abort               # 取消当前合并，重建合并前状态
$ git merge dev -Xtheirs          # 以合并dev分支到当前分支，有冲突则以dev分支为准
$ git rebase <branch>             # 衍合指定分支到当前分支
```

## 远程操作

```git
复制代码
$ git remote -v                   # 查看远程版本库信息
$ git remote show <remote>        # 查看指定远程版本库信息
$ git remote add <remote> <url>   # 添加远程版本库
$ git remote remove <remote>      # 删除指定的远程版本库
$ git fetch <remote>              # 从远程库获取代码
$ git pull <remote> <branch>      # 下载代码及快速合并
$ git push <remote> <branch>      # 上传代码及快速合并
$ git push <remote> :<branch/tag-name> # 删除远程分支或标签
$ git push --tags                 # 上传所有标签
```

## 打包

```git
$ git archive --format=zip --output ../file.zip master    # 将master分支打包成file.zip文件，保存在上一级目录
$ git archive --format=zip --output ../v1.2.zip v1.2      # 打包v1.2标签的文件，保存在上一级目录v1.2.zip文件中
$ git archive --format=zip v1.2 > ../v1.2.zip             # 作用同上一条命令
```

## 远程与本地合并

```git
$ git init                              # 初始化本地代码仓
$ git add .                             # 添加本地代码
$ git commit -m "add local source"      # 提交本地代码
$ git pull origin master                # 下载远程代码
$ git merge master                      # 合并master分支
$ git push -u origin master             # 上传代码
```

参考资料：<u>https://www.cnblogs.com/ldj3/p/9172804.html</u>
        <u>https://www.zhihu.com/question/41667536/answer/486640083</u>