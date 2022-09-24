---
title: "最大公约数"
date: 2022-08-05
lastmod: 2022-08-05
author: ["Quwny"]
tags:
- 最大公约数
- 辗转相除法
- 更相减损术
draft: false
---

最大公约数（Greatest Common Divisor），也称最大公因数、最大公因子，指两个或多个整数共有约数中最大的一个。

**a, b** 的最大公约数记为 `(a, b)` 。同样的，**a, b, c** 的最大公约数记为 `(a, b, c)`。

求最大公约数有多种方法，常见的有**质因数分解法、短除法、辗转相除法（欧几里德算法）、更相减损法**。

详细介绍可查看百度百科：[最大公约数](https://baike.baidu.com/item/%E6%9C%80%E5%A4%A7%E5%85%AC%E7%BA%A6%E6%95%B0/869308)

## 辗转相除法（欧几里德算法）

欧几里得算法又称辗转相除法，是指用于计算两个**非负整数 a, b 的最大公约数**。

定理：两个整数的最大公约数 等于 **其中较小的那个数和两数相除余数的最大公约数**。

计算公式 `gcd(a,b) = gcd(b,a mod b)`。

代码实现

```C
// 辗转相除法
int gcd(int a, int b) {
    while (b != 0) {
        // 余数
        int r = a % b;
        a = b;
        b = r;
    }
    return a;
}

// 辗转相除法 - 递归写法
int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}
```

> 注：C++17 中提供了库函数 `std::gcd`，定义于头文件 `<numeric>` 中，可以直接调用使用。

### 更相减损法

代码实现

```C
// 更相减损法
int gcd(int a, int b) {
    while (a != b) {
        if (a > b) a -= b;
        else b -= a;
    }
    return a;
}
```
