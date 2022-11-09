---
title: "SVN 笔记"
date: 2022-11-09T21:53:36+08:00
lastmod: 2022-11-09T21:53:36+08:00
author: ["Quwny"]
categories: 
- SVN
tags: 
- SVN
description: ""
weight: # 输入1可以顶置文章，用来给文章展示排序，不填就默认按时间排序
slug: ""
comments: true
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true # 顶部显示当前路径
draft: false # 草稿
---

## SVN 简介

Apache Subversion 简称 SVN ，是一个版本控制器。

## 相关概念

仓库（版本库）

## 服务端操作

```sh
# 创建版本库
svnadmin create <版本库路径（绝对路径）>

# 启动服务器 -- 默认占用 3690 端口
svnserve -d -r <版本库路径>

# 注册 Windows 服务 -- 方便使用
# 多仓库配置
sc create <服务名> binpath= "<服务路径>\svnserve.exe --service -r <版本库路径>" start= auto depend= Tcpip
```

### 权限

配置路径：`<版本库路径>\conf` 中的文件 `svnserve.conf` 修改访问权限

```sh
# write -- 可写，read -- 只读，none -- 无法访问。
# 匿名访问权限
anon-access = none # 可写。read -- 只读，none -- 无法访问

# 用户授权
autn-access = write

# 指定保存用户信息（用户名和密码）的文件
password-db = passwd

# 指定保存授权信息的文件
authz-db = authz
```

## 客户端操作

### SVN 基本操作

```sh
# 检出
svn checkout <svn://服务器地址/仓库地址>
svn checkout svn://localhost/rep # 本地

# 更新
svn update <文件名>

# 添加
svn add <文件名>

# 提交
svn commit -m "提交信息" <文件名>
```

### 历史记录
