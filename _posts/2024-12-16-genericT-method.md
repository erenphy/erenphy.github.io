---
layout:     post
title:      Rust之泛型函数
subtitle:   函数参数多态
date:       2024-12-16
author:     汤汤
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Rust
    - 泛型函数
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
||接口    |Into  |Rust之From、Into接口（trait）|
|√  |接口的定义|trait|Rust之Error接口（trait）|
|√  |结构体  |fmt::Formatter|Rust之Error接口（trait）|
|☐ |泛型函数 |<T>  |Rust之泛型函数|


### Question:

### 泛型函数

+ 基本用法：
  + 泛型参数用`<T>`表示
  + 可以用<T: trait>为泛型类型增加约束

### 多态
#### 静态多态（泛型+trait约束）
+ 在编译时确定类型
+ 单态化、为每种类型生成独立的代码

###### Rust泛型与CPP模板函数

```cpp
#include <iostream>
#include <cmath>

// 定义矩形结构体
struct Rectangle {
    double width, height;
    // 矩阵面积计算逻辑
    double area() const {
        return width * height;
    }
};

// 定义圆形结构体
struct Circle {
    double radius;
    // 圆形面积计算逻辑
    double area() const {
        return M_PI * radius * radius;
    }
};

// 泛型函数，通过模板处理不同类型
template <typename Shape>
double calculate_area(const Shape& shape) {
    return shape.area();
}

int main() {
    Rectangle rect{10.0, 20.0};
    Circle circle{5.0};

    std::cout << "Rectangle area: " << calculate_area(rect) << std::endl;
    std::cout << "Circle area: " << calculate_area(circle) << std::endl;

    return 0;
}
```

```rust
// 与CPP不同的是
// rust中需要显示定义一个 Trait 表示共有行为：这里是area()
trait Shape {
    fn area(&self) -> f64;
}

// 定义矩形结构体
struct Rectangle {
    width: f64,
    height: f64,
}
// 为矩形结构体实现Shape接口
impl Shape for Rectangle {
    fn area(&self) -> f64 {
        self.width * self.height
    }
}

// 定义圆形结构体
struct Circle {
    radius: f64,
}
// 为圆形结构体实现Shape接口
impl Shape for Circle {
    fn area(&self) -> f64 {
        std::f64::consts::PI * self.radius * self.radius
    }
}

// 泛型函数，接受任何实现了 Shape trait 的类型
// 这里的<T: Shape>是接口限制trait bounds。
// 它的作用是限制泛型T必须实现了Shape接口（impl Shape for some_T）
fn calculate_area<T: Shape>(shape: &T) -> f64 {
    shape.area()
}

fn main() {
    let rect = Rectangle {
        width: 10.0,
        height: 20.0,
    };
    let circle = Circle { radius: 5.0 };

    println!("Rectangle area: {}", calculate_area(&rect));
    println!("Circle area: {}", calculate_area(&circle));
}
```

+ 🛎️🛎️🛎️区别：

| 特性            | C++ 实现  | Rust 实现   |
|---------------|----------|---------------|
| 静态多态机制   | 模板（`template`） | 泛型和 trait（`<T: Trait>`）|
| 类型约束 |隐式约束，依赖编译器报错检查|显式约束，必须声明 `trait` |
| 错误发现 |延迟到调用时   |编译时即验证类型实现是否符合约束|
| 代码生成 |为每种类型实例化模板，单态化生成代码 |同样通过单态化优化性能|
| 接口定义的灵活性| 更灵活，但易导致模板膨胀或模糊错误| 明确接口，限制更严格但更安全|

> `trait bounds`   
> 确保泛型T实现了某个接口（impl any_Trait for some_T）  
> `<T: Shape + Display>` 支持使用`+`运算符执行多个bounds  

#### 动态多态（将泛型换成&dyn trait或Box<dyn Trait>）
+ 在运行时才确定类型
+ 动态分配
+ 可以处理不同类型的实例
+ 适用于多态集合等动态场景
