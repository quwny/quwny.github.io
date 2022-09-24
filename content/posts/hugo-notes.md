---
title: "关于使用 Hugo 搭建网站的一些事"
date: 2022-08-05
lastmod: 2022-08-12
author: ["Quwny"]
categories: 
- Hugo
tags: 
- Hugo
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true # 顶部显示当前路径
---

**Hugo 文档：[英文版](https://gohugo.io/)**

## 本地启动博客

在命令行中输入以下命令：

```sh
hugo server -D
```

## 创建文件

方式一：直接创建在 `content` 目录下。格式如下：

```sh
hugo new filename.md
# 如果文件名有空格，需要使用 双引号（""） 括起来。
hugo new "filename.md"
```

方式二：创建到指定的目录下（content 目录下的指定目录）。格式如下：

```sh
hugo new posts/filename.md
# 如果文件名有空格，需要使用 双引号（""） 括起来。
hugo new "posts/filename.md"
```
