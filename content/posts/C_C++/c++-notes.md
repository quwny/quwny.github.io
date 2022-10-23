---
title: "C++ 笔记"
date: 2022-08-12
lastmod: 2022-08-12
author: ["Quwny"]
categories: 
- C++
tags: 
- C++
draft: true
---

C++ 参考手册：[中文版](https://zh.cppreference.com/) [英文版](https://en.cppreference.com)

## C++ 编译器

C++ 编译器种类繁多，主要流行的几种：

* Windows 平台
  * MinGW: Minimalist GNU for Windows 的缩写，使 GCC 编译器支持 Windows 平台。
  * MSVC:
  * Clang llvm:
* Linux
  * Gcc
  * Clang llvm
* Mac
  * Gcc
  * Clang Apple
  * Clang llvm

## 关键字

### explicit

C++ 中的关键字 `explicit` 主要是用来修饰类的构造函数，被修饰的构造函数的类，**不能发生相应的隐式类型转换，只能以显示的方式进行类型转换**。类构造函数默认情况下声明为隐式的即 `implicit。`

## 循环语句

```C++
// 正式的语法 -- formal syntax
for (初始化语句（可选）; 迭代表达式（可选）) 语句


// 非正式的语法
for (声明或者表达式（可选）; 声明或者表达式（可选）; 表达式（可选）) 语句

// 样例
for (int i = 0; i < 10; ++i) {
  cout << i << endl;
}
```

## STL

### vector 容器

> 以下内容主要做简单的说明和一些个人觉得要记录的内容，详情请参考 [std::vector - C++ 参考手册](https://zh.cppreference.com/w/cpp/container/vector)

#### 简介

vector 数据结构和**数组**非常相似，也称为**单端数组**。vector 和普通数组的区别在于：数组是静态空间，而 vector 可以**动态扩展（动态扩展并不是在原空间之后续借新空间，而是找更大的内存空间，然后将原数据拷贝到新空间，释放原空间）**。

#### vector 构造器及初始化

```C++
// 构造器
vector(); // 默认构造函数
vector(size_type count); // 构造拥有个 count 默认插入的 T 实例的容器
vector(size_type count, const T& value = T()); // 构造拥有 count 个有值 value 的元素的容器

// 初始化

// 二维 vector 初始化
vector<vector<T>> v; // 调用默认构造函数
vector<vector<T>> v(n); // 构造 n 行 二维 vector
vector<vector<T>> v(n, vector<T>(m)); // 构造 n 行 m 列的二维 vector 且值为默认值
vector<vector<T>> v(n, vector<T>(m, data)); // 构造 n 行 m 列的二维 vector 且值为 data

// 使用 {} 初始化 -- Variable braced initialization
vector<vector<T>> v{}; // 初始化空的二维 vector
vector<vector<T>> v{{}}; // 初始化空的二维 vector
```

## 常用库函数

### `<numeric>` 标头

`std::iota` C++11 起，用从起始值开始连续递增的值填充一个范围。

示例

```C++
vector<int> vals(n);
iota(vals.begin(), vals.end(), 0);

int nums[n];
iota(nums, nums + n, -1);
```

详细定义：[iota(C++ 参考手册)](https://zh.cppreference.com/w/cpp/algorithm/iota)
