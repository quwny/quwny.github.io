---
title: "Java 笔记"
date: 2022-11-06T19:51:43+08:00
lastmod: 2022-11-06T19:51:43+08:00
author: ["Quwny"]
categories: 
- Java
tags: 
- Java
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

## 官方参考文档

[Java 参考文档](https://docs.oracle.com/en/java/javase/) 在该页面中可以找到对应 Java 版本的文档。

## 优先队列

Java 17 中的优先队列构造函数

![优先队列的构造函数](PriorityQueue_Constructor.png)

```Java
// 创建一个按照自然排序的队列（小根堆）
PriorityQueue<Integer> pq = new PriorityQueue<Integer>();

// 自定义排序（eg: 从大到小，大根堆）
PriorityQueue<Integer> pq = new PriorityQueue<Integer>((o1, o2) -> o2 - o1);
```
