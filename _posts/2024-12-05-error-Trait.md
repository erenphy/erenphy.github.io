---
layout:     post
title:      Rust之Error接口(trait)
subtitle:   error
date:       2024-12-04
author:     汤汤
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Rust
    - 错误处理
    - Some
    - Result
    - Option
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
|☐ |接口|Error |Rust之Error接口（trait）|

### Question:Error接口和Result里的Err枚举变体有啥关系

# Error
