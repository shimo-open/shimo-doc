# Git命令简介
准备工作: 如果你没有github账号, 您需要申请一个Github账号, 接下来可以继续下一步。

## 1 Fork 代码
1. 访问 [https://github.com/shimo-open/shimo-doc](https://github.com/shimo-open/shimo-doc)
2. 点击 "Fork" 按钮 (位于页面的右上方)

## 2 Clone 代码
一般我们推荐将origin设置为官方的仓库，而设置一个自己的upstream。

如果已经在github上开启了SSH，那么我们推荐使用SSH，否则使用HTTPS。两者之间的区别在于，使用HTTPS每次推代码到远程库的时候，都需要输入身份验证信息。
而我们强烈建议，官方库永远使用HTTPS，这样可以避免一些误操作。

```bash
git clone https://github.com/shimo-open/shimo-doc.git
cd shimo-doc
git remote add upstream 'git@github.com:<your github username>/shimo-doc.git'
```
upstream可以替换为任何你喜欢的名字。比如说你的用户名，你的昵称，或者直接使用me。后面的命令也要执行相应的替换。

## 3 同步代码
除非刚刚把代码拉到本地，否则我们需要先同步一下远程仓库的代码。
git fetch

在不指定远程库的时候，这个指令只会同步origin的代码。如果我们需要同步自己fork出来的，可以加上远程库名字：
git fetch upstream

## 4 创建 feature 分支
我们在创建新的 feature 分支的时候，要先考虑清楚，从哪个分支切出来。
我们假设，现在我们希望添加的特性将会被合并到master分支，或者说我们的新特性要在master的基础上进行，执行：
```bash
git checkout -b feature/my-feature origin/master
```
这样我们就切出来一个分支了。该分支的代码和origin/master上的完全一致。

## 5 提交 commit
```bash
git add .
git commit
git push upstream my-feature
```

## 6 提交 PR
访问 [https://github.com/shimo-open/shimo-doc](https://github.com/shimo-open/shimo-doc)
点击 "Compare" 比较变更并点击 "Pull request" 提交 PR

## 7 文档SEO
---
title: 给搜索引擎看的标题；可能和正文标题不一样
description: 关于此页的简短描述
image: 在社交媒体卡片中显示的缩略图

keywords: [描述, 中心的, 关键词]
---
[https://docusaurus.io/zh-CN/docs/seo](https://docusaurus.io/zh-CN/docs/seo)