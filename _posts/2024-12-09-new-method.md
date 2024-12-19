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
|√  |枚举    |Option|Rust之option、result枚举类型|
|√  |枚举    |Result|Rust之option、result枚举类型|
|√  |枚举变体|Some  |Rust之option、result枚举类型|
|√  |枚举变体|Ok    |Rust之option、result枚举类型|
|√  |枚举变体|Err   |Rust之option、result枚举类型|
|√  |关键字  |match |Rust之match、let、lf-let关键字|
|√  |关键字  |let   |Rust之match、let、lf-let关键字|
|√  |关键字  |if let|Rust之match、let、lf-let关键字|
|√  |接口    |Error |Rust之Error接口（trait）|
|√  |接口    |From  |Rust之From接口（trait） |
|√  |接口    |Into  |Rust之From、Into接口（trait）|
|√  |接口的定义|trait|Rust之Error接口（trait）|
|☐ |结构体  |fmt::Formatter|Rust之Error接口（trait）|
|√ |泛型函数 |<T>  |Rust之泛型函数|
|☐ |关联函数|new()  |Rust之关联函数、构造函数、方法与接口|
|☐ |关联函数|default|Rust之关联函数、构造函数、方法与接口|
|☐ |关联函数|from   |Rust之关联函数、构造函数、方法与接口|


### Question:接口和方法区别具体在哪里？什么是关联函数？
> 带着这个问题一起来看  

### 关联函数
> 定义在某个类型（结构体、枚举等）上的函数，不需要通过实例去调用

+ 使用方法：`TypeName::function_name()`
+ 常见例子
  + 构造函数new()
  + 与类型相关的实用工具函数
  + 
+ 与`方法method`的区别
  + 方法需要通过实例调用（通常第一个参数是`self`，表示方法是作用在某个具体实例上的）
  + 关联函数不需要`self`参数，不与具体实例绑定
  + ⭐⭐⭐特别的，构造函数一般用来创建实例；实例从而可以调用方法。

#### new()构造函数
###### 1. 示例
```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    // 一个关联函数，用来构造新的 Rectangle
    fn new(width: u32, height: u32) -> Self {
        Self { width, height }
    }
    
    // 一个普通方法，用来计算面积
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    // 调用关联函数来创建实例
    let rect = Rectangle::new(30, 50);
    
    // 调用实例方法
    println!("Area of rectangle: {}", rect.area());
}
```

###### 2. 为什么使用构造函数
> 一个直截了当的回答是，使用构造函数为某个类型创建实例，实现实例的初始化。  
> 但，这里有一个问题，只能使用new()构造函数实现结构体等类型的初始化吗？  
> 当然不是。  

1. 直接初始化各个字段创建结构体实例。假设有一个`Point`结构体：`let p = Point{x: 10, y: 20};`
2. 使用构造函数
   1. :bulb:结构体提供默认值（或结构体初始化逻辑复杂）的情况下，使用new()就可以不用手动设置每个字段
   2. 更进一步:bulb::bulb:，可以封装初始化逻辑，防止初始化时传入非法值

   ```rust
   fn new(width: u32, height: u32) -> Self {
        assert!(width > 0 && height > 0, "Width and height must be positive");
        Self { width, height }
    }
   ```
###### 3. 构造函数只能是new()吗:工厂方法
> :star2: new是构造函数指定关键字吗？  
> 其实不是，new只是一个惯例，rust社区遵循了这个约定而已  
> 除此之外，开发者问了提高语义与可读性，可以自定义构造函数名字。比较常见的有
`from` `default`等

1. 多个构造函数示例 

```rust
struct Shape {
    width: u32,
    height: u32,
}

impl Shape {
    fn square(size: u32) -> Self {
        Self { width: size, height: size }
    }

    fn rectangle(width: u32, height: u32) -> Self {
        Self { width, height }
    }
}

fn main() {
    let square = Shape::square(10);
    let rectangle = Shape::rectangle(10, 20);
}
```

#### from 构造函数
> 将一种类型转换为另一种类型的构造函数  
#### default 构造函数
> 通过default特质实现默认构造函数

#### 其他关联函数
###### trait
```rust
trait Shape { // 一个shape接口，
    fn description() -> String; //定义了一个关联函数description，不依赖实例，也没有self参数
}

struct Circle; // 定义一个circle结构体

impl Shape for Circle { //为circle结构体实现shape接口
    fn description() -> String {
        String::from("I am a circle.")//是库函数里的一个关联函数，将切片类型&str转换为堆分配String
    }
}

fn main() {
    println!("{}", Circle::description());// 直接使用::语法调用关联函数，不需要创建实例
}
```
