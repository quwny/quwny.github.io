---
title: "Matlab 笔记"
date: 2022-10-23T19:29:11+08:00
lastmod: 2022-10-23T19:29:11+08:00
author: ["Quwny"]
categories: 
- Matlab
tags: 
- Matlab
description: ""
weight: # 输入1可以顶置文章，用来给文章展示排序，不填就默认按时间排序
slug: ""
comments: true
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true # 顶部显示当前路径
draft: flase # 草稿
---

### 变量命名规则

字母、数字、下划线，不能以数字开头。

### Matlab 调用优先级

从高到底：

* 变量
* 内建函数
* 子函数
* 私有函数
  * MEX 文件
  * P 文件
  * M 文件

### Matlab 数据类型

* 逻辑
* 字符
* 数字
  * int8 int16 int32 int64
  * uint8 uint16 uint32 uint64
  * single 单精度浮点数
  * double 默认类型
* cell
* 结构
* Scalar @

### 数字格式

```matlab
format [short | long | shortE | longE | bank | hex | rat]

format % 恢复默认格式
```

### 字符 和 字符串

```matlab
Character(char) 0 ~ 255 ASCII 单字符

% string 字符串，使用单引号或者双引号

% 连接字符
s = [s1 s2];
s = [s1; s2]; % 需要维度（长度）相同

str = 'aardvark';

'a' == str % 返回的是字符串中每个字符对应的结果

str(str == 'a') = '2' % 将对应逻辑值为 1 的位置赋值为 '2'
```

### 结构体

### cell {}

### 向量

```matlab
%% 向量
a = [1 2 3 4]; % 行向量
b = [1; 2; 3; 4]; % 列向量

a(i); % 取其中一个元素

a * b; % 内积：行向量×列向量
b * a; % 外积：列向量×行向量
```

### 矩阵

矩阵基本语法

```matlab
%% 矩阵
% 3 * 3 矩阵
c = [1 2 3; 4 5 6; 7 8 9];

c(i); % 取元素，按列顺序，下标从 1 开始。
c(row, col); % 取 row 行 col 列的元素
c(i) = [] % 修改矩阵的值
% 注：i row col 可以是向量或矩阵，取对应值
```

矩阵连接

```matlab
A = [B C];      % 行连接
A = [B; C];     % 列连接
```

矩阵运算

* 加 `+`
* 减 `-`
* 乘 `*`
* 除（逆） `/`
* 幂 `^`
* 点 `.` 可以和别的运算符结合
  * `.*` 矩阵元素对应相乘
  * `./` 矩阵元素对应相除
* 转置 `'`

特殊矩阵

```matlab
eye(n);         % n 阶单位矩阵
zeros(n, m);    % n × m 阶零矩阵
ones(n, m);     % n × m 阶 1 矩阵
diag(A);        % 对角矩阵，元素为 A
rand();         % TODO 生成随机元素的矩阵
linspace();     % TODO
```

其他相关函数

```matlab
max(A);         % 返回列最大元素
min(A);         % 返回列最小元素

max(max(A));    % 返回矩阵的最大元素
min(min(A));    % 返回矩阵的最小元素

sum(A);
mean(A);        % 平均值
sort(A);
sortrows(A);
size(A);
length(A);
find(A);
```

### : 冒号操作符

创建向量

```matlab
% 创建从 j 到 k 的向量
A = j : k;
A = [j : k];

% 创建从 j 到 k，以 i 为间隔的向量
A = j : i : k; % i 为步长
A = [j : i : k];
```

赋值或替代

```matlab
A(3, :);        % 表示第三行
A(3, :) = []    % 消去第三行 
```

#### 逻辑操作符

`< <= > >= == ~=（不等于） && ||`

### if 语句

```matlab
if condition
    statement;
else if condition
    statement;
else
    statement;
end
```

### switch 语句

```matlab
switch expression
case value1
    statement;
case value2
    statement;
otherwise
    statement;
end
```

### while 语句

```matlab
while expression
    statement;
end
```

### for 语句

```matlab
% increment 步长，可选
for variable = start : increment : end
    statement;
end
```

### 函数

```matlab
% 声明函数
function x = filename(parameter)
statement;
```

```matlab
functon [x y] = filename(parameter)
```

```matlab
functon filename(parameter)
```

```matlab
% 函数柄
f = @(x)
```

### 常用命令及说明

```matlab
% 使用 %，单行注释

%% 表示分块

%{
    块级注释
    多行注释
%}

% 语句末尾是否有 ';'
% 如果有，不会将计算的值存储到 ans
% eg: a = 10    b = 10;

% 命令行窗口：按方向键的上下键（↑ ↓）可以查看上一条或下一条记录
% 末尾加 ... 表示连接下一行，当字符太多时

clc % 清屏
clear variable % 清除指定变量 variable
clear % 清除工作空间的变量
who % 显示工作空间存在的变量（只显示变量名）
whos % 显示工作空间存在的变量信息

prod(1 : n); % n!

disp(); % 打印结果到终端

%% 计时 从 tic 到 toc
tic
statement;
toc
```

### Matlab 快捷键

* `Ctrl + R` 快速注释
* `Ctrl + T` 取消注释
* `Ctrl + I` 对选中的块智能缩进
