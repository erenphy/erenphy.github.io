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
|√  |枚举    |Option|Rust之option、result枚举类型|
|√  |枚举    |Result|Rust之option、result枚举类型|
|√  |枚举变体|Some  |Rust之option、result枚举类型|
|√  |枚举变体|Ok    |Rust之option、result枚举类型|
|√  |枚举变体|Err   |Rust之option、result枚举类型|
|√  |关键字  |match |Rust之match、let、lf-let关键字|
|√  |关键字  |let   |Rust之match、let、lf-let关键字|
|√  |关键字  |if let|Rust之match、let、lf-let关键字|
|☐ |接口    |Error |Rust之Error接口（trait）|
|☐ |结构体  |fmt::Formatter|Rust之Error接口（trait）|

### Question:Error接口和Result里的Err枚举变体有啥关系
> 带着这个问题一起来看  

# 结构体
### 标准库fmt::Formatter详解
> Formatter结构体内部的具体定义和实现未公开  
##### Formatter提供的方法
1. `width()`：获取字段的宽度。
2. `precision()`：获取字段的精度。
3. `align()`：获取对齐方式（如左对齐、右对齐、居中对齐）。
4. `write_str()`：写入原始字符串。
5. `write_char()`：写入字符。
###### 使用示例
+ `let width = f.width(); ` 获取字段宽度
+ `let precision = f.precision(); ` 获取精度

# 属性宏（Attribute Macros）
> 作用于结构体、枚举或者函数等特定代码
> 可以修改、（主要是）扩展代码块的行为；如`#[derive(DEBUG)]`支持自动生成调试接口（debug trait）
### 用法：#[...]
### 常见的属性宏
+ `#[derive]`自动生成特定的接口实现
    + 如`#[derive(debug)]` `#[derive(clone)]`
+ ️`#[test]`标记一个函数为单元测试
+ `#[cfg]`条件编译宏（<font color="#0F5733">暂时还没接触过，待补充</font>）
    + `#[cfg(target_os = "windows")]`看这个例子挺好理解的
+ `#[Error]` 由thiserror库提供，可以自动为错误类型实现`std::fmt::Debug` `std::fmt::Display` `std::error::Error`这些接口。在enum内部可以直接使用`#[error(...)]`定义错误格式。 

```rust
use thiserror::Error;

#[derive(Error, Debug)]
pub enum MyError {
    #[error("An error occurred with code {0}")]
    CodeError(i32),

    #[error("Invalid input: {0}")]
    InvalidInput(String),
}
```

+ `#[derive(Clone)]` 自动为类型生成`clone`方法。
  + 会得到一个新的实例，内容完全相同。
  + 前提是，类型中的所有字段都实现了clone。

### 我可以自定义属性宏吗
可以，可以借助`proc_macro`自定义。<font color="#0F00aa">太抽象了，暂时没看懂具体操作</font>

### #[derive(Debug)]详解



---
# 

# Error
> Rust中的Error接口表示一个错误对象。凡是实现了这个接口的类型都可以使用它描述错误

### 常见的标准库中的Error
+ `std::io::Error`I/O错误，如文件操作、网络通信等
+ `std::fmt::Error`格式化错误
+ `std::num::PaserIntError`数字解析错误<font color="#0F5733">暂时还没接触过，待补充</font>

### 我可以自定义Error接口吗
可以，可以借助`std::error::Error`接口自定义。包括以下步骤

##### 1. 定义接口类型：结构体or枚举类型
+ ✅使用结构体struct

```rust
struct Myerror{
    details: String,
}
```
+ ✅使用枚举enum

```rust
enum Myerror{
    IoError(std::io::Error),
    PaserError(Strinh),
}
```
###### 1.1 补充上关联函数
> 通常需要定义一个或多个关联函数，支持创建错误实例

```rust
impl MyError{
    fn io_error(e: std::io::Error) -> MyError{
        MyError::IoError(e)
    }
    fn paser_error(msg: &str) -> MyError{
        MyError::PaserError(msg.to_string())
    }
}
```

##### 2. 实现std::fmt::Display打印错误信息
+ ✅`fmt::Display`接口面向用户提供格式化输出，通常是`println!`或`format!`
+ ✅`impl fmt::Display for MyError`为自己的`Myerror`类型实现`fmt::Display`接口，就可以用它打印格式化的错误详细信息了！  

> 🛎️这里与“`Display`接口提供用户友好的格式化输出” 相反的是`Debug`接口，它提供开发者友好的输出样式。

+ `fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result{}`是核心方法
    + `&self`表示当前的`Myerror`实例
    + `f: &mut fmt::Formatter<'_>`表示一个格式化器
        + `fmt::Formatter`是一个结构体，定义如何将内容格式化为字符串
        + `&mut` 可变引用，（格式化器需要动态构建字符串）
        + `'_` 生命周期标注

```rust
use std::fmt;

impl fmt::Display for MyError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "{}", self.message)
    }
}

```
##### 3. 实现std::fmt::Debug
+ 借助`#[derive(Debug)]`可自动化实现

##### 4. 实现std::error::Error
> ✅`std::error::Error`是一个接口，表示通用的错误类型。  
> ✅`impl std::error::Error for MyError`为自己的`MyError`类型实现标准库`std::error::Error`接口，进一步就可以用来表示自定义的错误类型啦

```rust
impl std::error::Error for MyError {
    fn source(&self) -> Option<&(dyn std::error::Error + 'static)> {
        // source函数支持链式错误处理
        // 如果 `MyError` 包含另一个错误，可以返回该错误
        None
    }
}
```

### 我怎么使用自定义的Error呢
```rust

fn test_func() -> Result<(), MyError>{
    Err(MyError::new("i don't know but something going wrong"))
}
```

### anyhow::Error
+ `anyhow`提供了一个*类型别名*`Result<T>`表示`Result<T, anyhow::Error>`。
    + 使用这个别名，需要先`use anyhow::{Result, Context};`