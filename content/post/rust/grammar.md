---
title: "Rust奇奇怪怪的语法"
date: 2021-01-28T19:26:32+08:00
tags: ["Rust"]
categories: ["Rust"]
draft: false
---

Rust 语法奇奇怪怪的写法。

`?` 操作符 （Error propagation）
======

函数调用后带 `?` 什么含义

```
 File::create(filename)?;
```
等同于

```
let output = match File::create(filename){
    Ok(f) => { f }
    Err(e) => { return Err(e); }
};
```

`?`操作符用于简化Result结构的写法，简化判断`Ok`和`Err`。

`||` 闭包结构
======

`|param1, param2| { // do something }` 结构

Rust闭包表达式写法,采用了与Smalltalk 和 Ruby 类似的`|`结构

```
let expensive_closure = |num| {
    println!("this is closure expensive...");
    num
};
```

两个竖线(`|`)包裹的是闭包参数，大括号(`{}`)内是闭包执行体, 单行时大括号可省略。


`_` 下划线操作符
======

* 忽略某些值
   
   类似于golang 中的用法
```
let numbers = (2, 4, 8, 16, 32);

match numbers {
    (first, _, third, _, fifth) => {
        println!("Some numbers: {}, {}, {}", first, third, fifth)
    },
}

// output: Some numbers: 2, 8, 32
```


* 变量名前添加`_` 用来忽略未使用的变量。

    以下划线开始变量名以便去掉未使用变量警告

```
fn main() {
    let _x = 5;   // _x不会产生警告
    let y = 10;  // y 未使用，会产生警告
}
```

* `_` 不会变量绑定

```
let s = Some(String::from("Hello!"));

// s 的值仍然会移动进 _s，并阻止我们再次使用 s
// if let Some(_s) = s {
// 只使用下`_`本身，并不会绑定值,s 没有被移动进`_`

if let Some(_) = s {
    println!("found a string");
}

println!("{:?}", s);
```

`r#` 操作符
======

原始标识符（Raw identifiers）前缀 `r#`。

原始标识符允许我们使用任何的单词作为标识符。

Rust不允许使用Rust定义的关键字以及保留字（例：use、as、type、where、match等）作为标识符。

```rust
fn match(needle: &str, haystack: &str) -> bool {
    haystack.contains(needle)
}
```
会得到如下错误:
```
error: expected identifier, found keyword `match`
 --> src/main.rs:4:4
  |
4 | fn match(needle: &str, haystack: &str) -> bool {
  |    ^^^^^ expected identifier, found keyword
```

正确写法

```rust
fn r#match(needle: &str, haystack: &str) -> bool {
    haystack.contains(needle)
}

fn main() {
    assert!(r#match("foo", "foobar"));
}
```




# 参考资料

[Rust 程序设计语言](http://120.78.128.153/rustbook/appendix-02-operators.html)
