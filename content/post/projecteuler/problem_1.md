---
title: "Multiples of 3 and 5"
date: 2020-10-28T17:41:25+08:00
draft: true
tags: ["欧拉计划", "数学"]
categories: ["欧拉计划"]
---

Multiples of 3 and 5
=======

If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.
Find the sum of all the multiples of 3 or 5 below 1000.

---

#### 3或5的倍数

在小于10的自然数中，3或5的倍数有3、5、6和9，这些数之和是23。

求小于1000的自然数中所有或的倍数之和。

---


## 知识点

等差数列求和公式

$$
    \sum_{k=1}^{n} = \frac{n * (n + 1)}{2}
$$

$$
    \sum_{k_{1}=1}^{333} *3k_{1} + \sum_{k_{2}=1}^{199} * 5k_{2} - \sum_{k_{3}=1}^{66} * 15k_{3} = 166833 + 99500 − 33165 = 233168
$$



## 实现

### C

```C
#include <stdio.h>

int arithmetic_progression_sum(int i, int limit) {
	int len = (limit - 1) / i;
	return i * len * (len + 1) / 2;
}

int main(void){
    int s3  = arithmetic_progression_sum(3, 1000);
    int s5  = arithmetic_progression_sum(5, 1000);
    int s15 = arithmetic_progression_sum(15, 1000);
    int result = s3 + s5 - s15;
    printf("%d",result);
    return 0;
}

```

### Java

```Java
class Main {
    public static void main(String[] args) {
        int s3 = arithmeticProgressionSum(3, 1000);
        int s5 = arithmeticProgressionSum(5, 1000);
        int s15 = arithmeticProgressionSum(15, 1000);
        int result = s3 + s5 - s15;
        System.out.println(result);
    }

    private static int arithmeticProgressionSum(int i, int limit) {
        int len = (limit - 1) / i;
        return i * len * (len + 1) / 2;
    }
}
```

### Go

```Golang
package main

import (
	"fmt"
)

func main() {
	s3 := arithmetic_progression_sum(3, 1000)
	s5 := arithmetic_progression_sum(5, 1000)
	s15 := arithmetic_progression_sum(15, 1000)
	result := s3 + s5 - s15
	fmt.Println(result)
}

func arithmetic_progression_sum(i uint32, limit uint32) uint32 {
	len := (limit - 1) / i
	return i * len * (len + 1) / 2
}

```


### Rust

```Rust

fn main() {
    let s3 = arithmetic_progression_sum(3, 1000);
    let s5 = arithmetic_progression_sum(5, 1000);
    let s15 = arithmetic_progression_sum(15, 1000);
    let result = s3 + s5 - s15;
    println!("{}", result);
}

fn arithmetic_progression_sum(i: u32, limit: u32) -> u32 {
    let len = (limit - 1) / i;
    i * len * (len + 1) / 2
}

```





