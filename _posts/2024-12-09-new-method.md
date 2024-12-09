---
layout:     post
title:      Rust之关联函数、构造函数、方法与接口
subtitle:   new构造函数、method方法的区别
date:       2024-12-09
author:     汤汤
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Rust
    - new
    - method
    - trait
    - 静态方法
    - 关联函数
    - Note
    - cpp
---
> 来自 [black-hat-rust](https://github.com/skerkour/black-hat-rust) 

|   |--  |名称|导航|
|:-:|:-----:|:--------------:|:---:|
|🗹 |枚举    |Option|Rust之option、result枚举类型|
|🗹 |枚举    |Result|Rust之option、result枚举类型|
|🗹 |枚举变体|Some  |Rust之option、result枚举类型|
|🗹 |枚举变体|Ok    |Rust之option、result枚举类型|
|🗹 |枚举变体|Err   |Rust之option、result枚举类型|
|🗹 |关键字  |match |Rust之match、let、lf-let关键字|
|🗹 |关键字  |let   |Rust之match、let、lf-let关键字|
|🗹 |关键字  |if let|Rust之match、let、lf-let关键字|
|☐ |接口    |Error |Rust之Error接口（trait）|
|☐ |结构体  |fmt::Formatter|Rust之Error接口（trait）|
|☐ |关联函数|new   |Rust之关联函数、构造函数、方法与接口|
|☐ |接口定义|trait |Rust之关联函数、构造函数、方法与接口|



### Question:接口和方法区别具体在哪里？什么是关联函数？
> 带着这个问题一起来看  

