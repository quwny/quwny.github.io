---
title: "Java中的Lambda表达式"
date: 2022-04-30T16:22:44+08:00
lastmod: 2022-04-30T16:22:44+08:00
author: ["Quwny"]
categories: 
- Java
tags: 
- Java
- Lambda
- 学习笔记
description: "学习笔记：介绍 Java 中的 Lambda 表达式"
weight: # 输入1可以顶置文章，用来给文章展示排序，不填就默认按时间排序
slug: ""
comments: true
showToc: true # 显示目录
TocOpen: false # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true #顶部显示当前路径
cover:
    image: ""
    caption: ""
    alt: ""
    relative: false
draft: false
---

参考视频：[尚硅谷Java入门视频教程](https://www.bilibili.com/video/BV1Kb411W75N)

## Lambda 简介

Java 8 中引入了一个新的操作符 `->` ，该操作符称为 **箭头操作符 或 Lambda 操作符**。
箭头操作符将 Lambda 表达式拆分为两部分：`( 参数列表 ) -> { Lambda 体 }`

## Lambda 语法格式

语法格式一：**无参数，无返回值**：`() -> {}`

```java
() -> System.out.println("hello Lambda!");
```

语法格式二：**有一个参数，无返回值**（若只有一个参数，小括号可以省略不写）： `(x) -> {}`

```java
Consumer<String> consumer = (x) -> System.out.println(x);
Consumer<String> consumer = x -> System.out.println(x);
```

语法格式三：**有两个以上的参数，有返回值，并且 Lambda 体中有多条语句**

```java
Comparator<Integer> comparator = (x, y) -> {
    System.out.println("函数式接口");
    return Integer.compare(x, y);
};
```

语法格式四：**若 Lambda 体中只有一条语句，return 和 大括号都可以省略不写**

```java
Comparator<Integer> comparator = (x, y) -> Integer.compare(x, y);
```

语法格式五：**Lambda 表达式的参数列表的数据类型可以省略不写**，JVM编译器可以通过上下文推断出数据类型，即“**类型推断**”

```java
(Integer x, Integer y) -> Integer.compare(x, y);
(x, y) -> Integer.compare(x, y);
```
