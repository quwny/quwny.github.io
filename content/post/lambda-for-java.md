---
title: "Java 中的 Lambda 表达式"
date: 2022-04-30
lastmod: 2022-04-30
author: ["Quwny"]
categories: 
- Java
tags: 
- Java
- Lambda
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
