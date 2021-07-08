---
title: "你必须知道的位运算"
date: 2021-06-04T18:35:04+08:00
tags: ["位运算"]
categories: ["位运算"]
draft: true
---

## 检测两个整数是否符号相反

`^` (异或）两值不相同结果为 1。

正数和负数最高位（符号位）值不同(正数为0，负数为1)，最高位异或操作后，若符号不同，则最高为`1`，结果即为负数。

```java
    int x = 6;
    int y = -6;
    boolean result = (x ^ y) < 0;
    System.out.println(result ? "opposite" : "same");
```

> 正数的二进制以原码方式表示，负数以正数的补码方式表示。

## 检测一个正整数是否为2的幂

如果一个正整数为2的幂，那么其二进制位中只有一位位1。以 8 为例。

```java
    System.out.println(Integer.toBinaryString(8));        // 1000
    System.out.println(Integer.toBinaryString(8 - 1));    // 111
```

8 二进制表示:    `0000_1_0_0_0`

8-1 二进制表示:  `0000_0_1_1_1`

`&`（相与）之后为 0。

`(v & (v - 1)) == 0` 则可以判断v是否为2的幂


```java
    for (int i = 0; i <= 16; i++) {
         boolean r = i > 0 && (i & (i - 1)) == 0;    // 注意 0不是2的幂 
         System.out.printf("%d is a power of 2: %s %n", i, r ? "yes" : "no");
    }
```




## leetcode题


### 136. 只出现一次的数字

[136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

> 给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。











## 参考

[Bit Twiddling Hacks](http://graphics.stanford.edu/~seander/bithacks.html)

[Matters Computational: Ideas, Algorithms, Source Code](https://www.jjj.de/fxt/fxtbook.pdf)


https://www.cnblogs.com/inmoonlight/p/9301733.html


http://ponder.work/2020/08/01/variable-precision-SWAR-algorithm/

https://zhuanlan.zhihu.com/p/37014715
