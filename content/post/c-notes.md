---
title: "C 语言笔记"
date: 2022-08-12T19:23:50+08:00
lastmod: 2022-08-12T19:23:50+08:00
author: ["Quwny"]
categories: 
- C 语言
tags: 
- C
draft: true
---

**C 参考手册：[中文版](https://zh.cppreference.com/w/c) [英文版](https://en.cppreference.com/w/c)**

## 关于 字符 和 sizeof

```C
sizeof('A'); // 结果为 4
char ch = 'A';
sizeof(ch); // 结果为 1
```

为何会有不同的结果？

**C99 标准规定，字符常量是 `int` 类型**。

## 字符串常用函数

```C
/**
 * dest 
 * src
 * return 
 */
char *strcpy( char *dest, const char *src ); // C99 起

/**
 * dest 
 * src
 * return 
 */
char *strcat( char *dest, const char *src ); // C99 起

size_t strlen( const char *str );
```
