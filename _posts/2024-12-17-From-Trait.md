---
layout:     post
title:      Rust之From、Into接口(trait)
subtitle:   From
date:       2024-12-17
author:     汤汤
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Rust
    - 关联函数
    - From
    - Into
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
|√  |接口    |Error |Rust之Error接口（trait）|
|√  |结构体  |fmt::Formatter|Rust之Error接口（trait）|
|☐ |接口    |From  |Rust之From、Into接口（trait）|
|☐ |接口    |Into  |Rust之From、Into接口（trait）|
|√  |泛型函数 |<T>  |Rust之泛型函数|


# From接口
+ Rust中的From接口用来定义类型之间的转换逻辑。  
+ from是From接口的关联函数，用于执行从一个类型到另一种类型的显式转换。

```rust
// T表示源类型，也就是要接受的参数类型
// Self表示目标类型
pub trait From<T> {
    fn from(value: T) -> Self;
}
```


### 常见的标准库中的From
+ 从`&str`转换为`String`: `let s = String::from("hello"); // 从 &str 转换为 String`
+ 数值类型转换

```rust
let x: u8 = 10;
let y: u32 = u32::from(x); // 从 u8 转换为 u32
```
+ 从数组到向量 `let v = Vec::from([1, 2, 3]); // 从数组转换为 Vec`

### Into接口
> 如果类型 T 实现了 From<U>，那么类型 U 就会自动实现 Into<T>。
> 换句话说，From 是基础，而 Into 是派生。

#### 如何使用Into
###### 自定义并实现某类型的From接口，自动派生Into
###### 也可以手动定义Into

#### 为什么要用Into--函数参数多态

```rust
fn send_message<T: Into<String>>(message: T) {
    let message: String = message.into(); // 使用 Into 接口将输入转换为 String
    println!("Sending message: {}", message);
}

fn main() {
    // 1. 传递 &str 类型
    send_message("Hello, world!");

    // 2. 传递 String 类型
    let msg = String::from("This is a String");
    send_message(msg);

    // 3. 传递可以转换为 String 的自定义类型
    struct User {
        name: String,
    }

    impl Into<String> for User {
        fn into(self) -> String {
            format!("User: {}", self.name)
        }
    }

    let user = User {
        name: String::from("Alice"),
    };
    send_message(user); // 自动调用 Into 接口，将 User 转换为 String
}
```


### 我怎么自定义某个类型的From接口
###### 1. 给出自定义类型的定义（如定义一个Person结构体）
###### 2. 为这个结构体实现From接口（impl From<&str> for Person）
###### 3. 通过定义关联函数from，定义类型转换的逻辑
```rust
struct Person {
    name: String,
    age: u8,
}

impl From<&str> for Person {
    fn from(s: &str) -> Self {
        let parts: Vec<&str> = s.split(',').collect();
        Person {
            // trim()用来去除空格
            // to_string()转换为堆分配的String
            name: parts[0].trim().to_string(),
            // parse()转换为U8类型
            // unwrap()是对parse函数的错误处理
            age: parts[1].trim().parse().unwrap(),
        }
    }
}

fn main() {
    // 在这里，类型 Person 实现了 From<&str> 特质，那么 &str 类型就会自动实现 Into<Person> 特质。
    // 因此，以下代码显式的调用&str的into()，实际上还是调用Person结构体的From接口
    let person: Person = "Alice, 30".into();
    println!("Name: {}, Age: {}", person.name, person.age);
}
```
