---
layout:     post
title:      Rust之option、result枚举类型
subtitle:   Option与Result枚举
date:       2024-12-01
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

<font color="#46aa00">Rust中，若预计某变量值在某时刻可能为空值，则尽量用Option/Result定义它</font>。为此设定了`enum Option`和`enum Result`两个枚举类型。

+ `enum Option`
  + `Some()`表示有返回值
  + `None()`
+ `enum Result`
  + `Ok()`表示正确返回
  + `Err()`表示返回错误

# Option枚举
> std::option
> 表示一个可选值

```rust
// 每个option要么是一个some中包含一个值
// 要么是一个None
pub enum Option<T>{
    None,
    Some(T),
}
// 由此，一旦某变量不是Option类型的 
// 我们则认为它的值永远不会是空  
```

#### 如何赋一个值给Option
举个:chestnut:  

```rust
fn main(){
  let a: i32 = 1;
  let b: Option<i32> = Some(5);
  // Option和T不是同一个类，不能直接进行运算
  let c = a + b; // 编译阶段就会报错
  // 应该先进行类型转换。  
  // 当option不为空时，unwrap方法拆解出option的值，并获得值的所有权  
  // 注意：若Option的值为空，unwrap()会直接触发panic  
  let cc = a + b.unwrap(); 
  println!("{}", c);
}
// 由此，符合了Rust的固有设定：在编译阶段就能发现错误  
```
#### 如何从Option取值
- :chestnut: `let val0: Option<i32> = get_some_val();` 
- 取值方法1：`match`
- 取值方法2：`if let`

```rust
match  val0{
  Some(num) => println!("val is: {}", num);
  None => println!("val is none");
}

if let Some(num) = val0{
  println!("val is: {}", num);
}else {
  println!("val is none");
}
// 这里，无论哪种方法，都需要主动去取Option的值，更不能直接用Option类
```
- 取值方法3：`unwrap()`方法，如第一个例子讲的那样，unwrap有值时返值，空时引发panic。

### 枚举成员Some
> 首先明确一点：  
> Some不是一个关键字，他只是枚举类型Option的一个枚举成员。

`Some(T)`用来包装一个实际的值，表示这个值是存在的。和None配合，实现安全处理可能存在空值的情况，避免空指针异常。

### 用处
1. 作为初始值
2. 返回值，用于报告，错误返回None
3. 可选的结构体字段
4. 可借用的结构体字段
5. 可选的函数参数
6. 可空指针

# Result枚举  
> std::result::Result
> Result很常用，直接被包含到`prelude`中
> 因此不需要手动引入这个包

```rust
enum Result<T, E>{// 泛型T表示正确执行后返回值的类型，E表示异常时存放的错误值 
  Ok(T), // 返回值的存放方式为Ok(T)
  Err(E),// 返回值的存放方式为Err(E)
}
```
#### 简单例子 
举一个读取文件的::chestnut:: 

```rust
use std::fs::file;

fn main(){
  let f = File::open("hello.txt");
  let f:u32 = File::open("hello.txt");//会报错，打开文件操作的返回值应该是一个文件句柄，再不济也是个IO错误。不能是整型
  // open函数需要告知调用者是否执行成功，并返回执行结果的。
  // 这些都通过Result实现。
  let f = match f {
    Ok(file) => file,
    Error(error) => {
      panic!("Something wrong in opening file:{:?}", error);
    },
  };
}
```
