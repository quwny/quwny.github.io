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

### nullptr

空指针

## 循环语句

```c++
for (声明或者表达式（可选）; 声明或者表达式（可选）; 表达式（可选）) 语句

// 样例
for (int i = 0; i < 10; ++i) {
  cout << i << endl;
}
```

## 常用库函数

### vector 容器

> 以下内容主要做简单的说明和一些个人觉得要记录的内容，详情请参考 [std::vector - C++ 参考手册](https://zh.cppreference.com/w/cpp/container/vector)

#### 简介

vector 数据结构和**数组**非常相似，也称为**单端数组**。vector 和普通数组的区别在于：数组是静态空间，而 vector 可以**动态扩展（动态扩展并不是在原空间之后续借新空间，而是找更大的内存空间，然后将原数据拷贝到新空间，释放原空间）**。

* `[]` 访问指定的元素
* push_back 在末尾添加元素
* pop_back 在末尾删除元素
* front 访问第一个元素
* back 访问最后一个元素

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

### `<algorithm>`

* count, count_if 返回满足指定判别标准的元素数
* fill 将一个给定值赋值给一个范围内的每个元素
* swap 交换两个对象的值
* reverse 逆转范围中的元素顺序
* unique 移除范围内的连续重复元素
* sort 将范围内元素进行排序（默认升序）
* max, min 返回给定值的最大，最小元素

### `<queue>`

queue 队列

* front, back 访问第一个，最后一个元素
* empty, size 判空，元素个数
* push, emplace, pop 队尾插入元素，删除队首元素

priority_queue 优先队列（默认大根堆）

* top 访问栈顶元素（返回引用）
* empty, size, push, pop 与queue操作类似



```c++
// 大根堆
priority_queue<int> pq;
priority_queue<int, vector<int>, less<int>> pq;

// 小根堆
priority_queue<int, vector<int>, greater<int>> pq;
```



### `<set>` 有序，键按字典序排序，`<unordered_set>` 无序 C++11

* 迭代器：begin, end
* empty, size
* clear 清楚内容
* insert 插入元素
* erase 擦除元素
* find 寻找键的元素，若找不到返回 end



遍历 set

使用迭代器

```c++
// for (set<int>::iterator it = m.begin(); it != m.end(); it++)
for (auto it = m.begin(); it != m.end(); it++) {
    // code
}

for (auto val : m) {
    // code
}
```



### `<map>` 键有序，`<unordered_map>` 键无序 C++11

* `[]` 访问、插入指定的元素
* at 访问指定的元素
* 其他基本操作同 set

```c++
for (auto it = m.begin(); it != m.end(); it++) {
    cout << it->first << ": " << it->second << endl;
}

for (auto it : m) {
    cout << it.first << ": " << it.second << endl;
}
```



### `<stack>` 栈

* top 栈顶元素
* empty, size, push, pop

## 对象和类

类作用域：在类中定义的名称（成员、函数）的作用域为整个类。



### 作用域内枚举（C++11）

在传统的枚举中，两个枚举定义的枚举量可能发生冲突。C++11 提供了一种新枚举，其枚举量的作用域为类。

```c++
// 定义，关键字 class 可以使用 struct 替换。
enum class e1 {val1, val2};
enum class e2 {val1, val2};

// 使用
e1::val1;
e2::val1;
```



枚举实际用某种底层整型类型表示，这取决于具体实现。可以指定底层类型。

```c++
enum class : short e1 {val1, val2}; // 指定底层类型为 short
```



### 使用类

运算符重载，语法格式：`operator op (argument-list)`，op 是有效的 C++ 运算符。

```c++
// 重载 + 运算符
Object operator+(const Object &a, const Object &b);

// 类对象相加
obj = obj1 + obj2;
// 当编译器编译时，使用相应的运算符函数替换运算符
obj = obj1.operator+(obj2);
```



### 友元

友元函数、友元类、友元成员函数

友元函数：将函数原型放在类声明中，并在原型声明前添加 `friend` 关键字。

```c++
class Object {
  friend Time operator*(double m, const Time &t);
};

// 在外部定义时，不添加 friend
Time operator*(double m, const Time &t) {
    // code
}
```

1. 友元函数拥有与成员函数相同的访问权限
2. 在定义中不添加 friend （除在原型中声明的同时定义）



常用的友元：重载  `<<` 运算符。一般来说，定义如下：

```c++
ostream &operator<<(ostream &os, const Object &obj) {
    os << ...;
    return os;
}
```



重载运算符：成员函数和非成员函数

在定义运算符时，如果匹配格式相同，必须选择其一，而不能同时选择，否则存在二义性错误。具体使用哪种，一般情况下无区别。根据类设计，非成员函数可能更好。



### 类的自动类型转换和强制类型转换

接受一个参数的构造函数能作为转换函数。

```c++
// 有如下构造函数
Object(int val);
Object(double val1, int val2 = 10);

// 类的自动类型转换
Object obj = 10; 	// int -> Object, obj = Object(10);
Object obj = 10.12; // double -> Object, obj = Object(10.12, 10);
```

以上案例为类的自动类型转换，使用关键字 `explicit` 可关闭上述的隐式转换。

构造函数只用于某种类型到类的转换。



转换函数

用户定义的强制类型转换，转换函数的形式：

```c++
operator typename(); // typename: int, double, Object, ...

operator int();
```

> 转换函数必须是类方法，不能指定返回类型，不能有参数



### 动态内存和类

不能在类声明中初始化静态成员变量。静态类成员可以在类声明之外使用单独的语句来进行初始化。

```c++
class Clazz {
    static int num;
    ...
};

int Clazz::num = 10; // 不用关联 static
```

> 如果静态成员是 const 整数类型或枚举类型，则可以在类声明中初始化。



### C++ 自动定义的成员函数

如果没有定义下列成员函数，C++ 会自动定义：

1. 默认构造函数
2. 默认析构函数
3. 复制构造函数
4. 赋值运算符
5. 地址运算符

复制构造函数，用于将一个对象复制到新创建的对象中。原型通常如下：

```c++
Object(const Object &);
```

当新建一个对象并将其初始化为同类对象时，或者按值传递对象或函数返回对象时，会调用复制构造函数。

默认的复制构造函数的功能：逐个复制非静态成员。（成员复制也称为浅复制），一般需要显式定义复制构造函数进行深复制。

---

赋值运算符，原型通常如下：

```c++
Object &operator=(const Ojbect &);
```



### 成员初始化列表

由前面带冒号（`:`）的，逗号（`,`）分隔的初始化列表组成，会在创建对象时进行初始化。

对于 const 数据成员、被声明为引用的类成员需要采用此种方式初始化。其他成员也可采用此方式。

```c++
class Object {
    const int mem1;
    int mem2;
    
public:
    Object(int arg) : mem1(arg), mem2(arg * 2) {}
};
```



C++11 运行在类初始化

```c++
class Object {
    int mem1 = 20;
    const int mem2 = 10;
};
```



### 类继承

一般形式：

```c++
class Object : [public | private | protected] Parent {
    // code
};
```

派生类构造函数可以使用初始化列表机制将值传递给基类构造函数。

继承方式有：公有（public）、保护（protected）、私有（private）



### 为何需要虚析构函数？

使用虚析构函数可以确保正确的析构函数序列被调用。

如果没有使用 virtual，程序将根据引用类型或指针类型选择方法。

如果使用了 virtual，程序将根据引用或指针指向的对象的类型来选择方法。



### 静态联编（早期联编）和动态联编（晚期联编）



### 类模板

```c++
template<class Type> // 或
template<typename Type>
```

