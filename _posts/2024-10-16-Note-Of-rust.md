<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [layout:     post
title:      black-hat-rust-chapter01
subtitle:   阅读笔记-第一章
date:       2024-10-16
author:     汤汤
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Rust
    - Note
    - cpp](#layout-----post%0Atitle------black-hat-rust-chapter01%0Asubtitle---%E9%98%85%E8%AF%BB%E7%AC%94%E8%AE%B0-%E7%AC%AC%E4%B8%80%E7%AB%A0%0Adate-------2024-10-16%0Aauthor-----%E6%B1%A4%E6%B1%A4%0Aheader-img-imgpost-bg-ios9-webjpg%0Acatalog-true%0Atags%0A------rust%0A------note%0A------cpp)
- [写在前面](#%E5%86%99%E5%9C%A8%E5%89%8D%E9%9D%A2)
- [chapter01](#chapter01)
    - [1. 攻击阶段](#1-%E6%94%BB%E5%87%BB%E9%98%B6%E6%AE%B5)
        - [侦察](#%E4%BE%A6%E5%AF%9F)
        - [利用](#%E5%88%A9%E7%94%A8)
        - [横向（lateral）移动](#%E6%A8%AA%E5%90%91lateral%E7%A7%BB%E5%8A%A8)
        - [数据渗透](#%E6%95%B0%E6%8D%AE%E6%B8%97%E9%80%8F)
        - [清理](#%E6%B8%85%E7%90%86)
    - [2. 攻击者简介](#2-%E6%94%BB%E5%87%BB%E8%80%85%E7%AE%80%E4%BB%8B)
        - [hacker](#hacker)
        - [exploit writer](#exploit-writer)
        - [developer](#developer)
        - [系统管理员](#%E7%B3%BB%E7%BB%9F%E7%AE%A1%E7%90%86%E5%91%98)
        - [分析者](#%E5%88%86%E6%9E%90%E8%80%85)
    - [3. Rust与其他语言的区别](#3-rust%E4%B8%8E%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%9A%84%E5%8C%BA%E5%88%AB)
    - [4. Awesome Rust](#4-awesome-rust)
      - [Setup](#setup)
        - [安装rustup](#%E5%AE%89%E8%A3%85rustup)
        - [cargo安装各类工具](#cargo%E5%AE%89%E8%A3%85%E5%90%84%E7%B1%BB%E5%B7%A5%E5%85%B7)
        - [配置编辑器](#%E9%85%8D%E7%BD%AE%E7%BC%96%E8%BE%91%E5%99%A8)
    - [5. 小试牛刀](#5-%E5%B0%8F%E8%AF%95%E7%89%9B%E5%88%80)
      - [5.1 代码框架](#51-%E4%BB%A3%E7%A0%81%E6%A1%86%E6%9E%B6)
      - [5.2 示例代码分析](#52-%E7%A4%BA%E4%BE%8B%E4%BB%A3%E7%A0%81%E5%88%86%E6%9E%90)
        - [5.2.1函数返回值 `Result<T,E>`](#521%E5%87%BD%E6%95%B0%E8%BF%94%E5%9B%9E%E5%80%BC-resultte)
        - [5.2.2 Box与dyn](#522-box%E4%B8%8Edyn)
        - [5.2.3 <font color="#0047a5">Trait对象与dyn</font>](#523-font-color0047a5trait%E5%AF%B9%E8%B1%A1%E4%B8%8Edynfont)
        - [5.2.4 泛型参数](#524-%E6%B3%9B%E5%9E%8B%E5%8F%82%E6%95%B0)
        - [5.2.5 RAII(资源获取时初始化)](#525-raii%E8%B5%84%E6%BA%90%E8%8E%B7%E5%8F%96%E6%97%B6%E5%88%9D%E5%A7%8B%E5%8C%96)
        - [5.2.6 Ok(())](#526-ok)
    - [6. For:初学者](#6-for%E5%88%9D%E5%AD%A6%E8%80%85)
      - [Rust的内存安全机制](#rust%E7%9A%84%E5%86%85%E5%AD%98%E5%AE%89%E5%85%A8%E6%9C%BA%E5%88%B6)
        - [所有权](#%E6%89%80%E6%9C%89%E6%9D%83)
        - [借用](#%E5%80%9F%E7%94%A8)
        - [生命周期](#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
        - [引用计数智能指针](#%E5%BC%95%E7%94%A8%E8%AE%A1%E6%95%B0%E6%99%BA%E8%83%BD%E6%8C%87%E9%92%88)
        - [维护与更新](#%E7%BB%B4%E6%8A%A4%E4%B8%8E%E6%9B%B4%E6%96%B0)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

---
layout:     post
title:      black-hat-rust-chapter01
subtitle:   阅读笔记-第一章
date:       2024-10-16
author:     汤汤
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Rust
    - Note
    - cpp
---
> 来自 [black-hat-rust](https://github.com/skerkour/black-hat-rust) 

## 写在前面
## chapter01
#### 1. 攻击阶段
###### 侦察
> 目的：获取目标尽可能多的信息
> 如，开发者名字、互联网上的机器数量、运行的服务...

+ 被动
  + 使用公开数据
+ 主动
  + 直接扫描目标网络

###### 利用
+ `0-day`漏洞或非`0-day`漏洞
+ 社会工程

###### 横向（lateral）移动
+ 目的：保持访问权限、获得更多资源和系统
+ 挑战：尽可能隐蔽
+ 工具：远程访问工具RATs

###### 数据渗透
> 通过网络传输大量数据，很容易被发现

###### 清理
+ logs
+ 临时文件
+ 钓鱼网站
+ 基础设施




#### 2. 攻击者简介
###### hacker
###### exploit writer
> 武器化

###### developer
+ 自定义攻击工具（代理、credential dumpers等）
+ 使用公共、现有工具

###### 系统管理员
> 一旦攻略“系统管理员”成功，即可保护攻击方的基础设施，也可帮助后期的`利用`和`横向移动`阶段

###### 分析者
> 解释攻击结果、分析目标的具体优先级

#### 3. Rust与其他语言的区别

|编程语言|问题|
|:--:|:--:|
| C,C++|不够安全|
|python|慢；弱类型导致难以编写大型程序|
|Java|依赖较长的运行时间|

#### 4. Awesome Rust
##### Setup
###### 安装rustup
> 借助rustup管理工具链  
> 可执行`rustup update`进行升级更新  
+ 安装方法
  + windows [下载地址](https://www.rust-lang.org/zh-CN/tools/install) 
  + 类unix安装命令
`curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`  
    + 但是老报错👀，可能压缩包太大了，网络状况堪忧，推荐下一种方法  
  
  + [官方链接](https://forge.rust-lang.org/infra/other-installation-methods.html)找到适合自己操作系统的压缩包下载、解压，然后运行`sudo ./install.sh`进行安装  
    + 安装包括编译器rustc\包管理器cargo\

+ 验证
  + 安装完成后，运行`rustc --version`
    + 这里的`rustc`是rust编辑器
    + 我的显示`rustc 1.82.0 (f6e511eec 2024-10-15)`，安装成功啦🥂🥂🥂

###### cargo安装各类工具
+ **换国内源**
  + 在`/home/.cargo/config.toml`中添加  
```toml
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"
```

+ 添加环境变量
  + `cargo install rustfmt`执行后，有一个warning：`warning: be sure to add `/home/lxh/.cargo/bin` to your PATH to be able to run the installed binaries`提示需要将bin加入环境变量
    + 查看当前环境变量`echo $PATH`
  + 方法1：`export PATH=/home/lxh/.cargo/bin:$PATH`
    + 缺点：一次性修改
  + 方法2：修改`~/.bashrc`文件，并终端执行`$ source ~/.bashrc`立即生效，且永久有效  
  + 方法3：修改`/etc/profile`，同2

###### 配置编辑器
+ 在vscode中装一个`rust-analyzer`插件，支持命令补全、代码检查
+ 待补充...

#### 5. 小试牛刀
##### 5.1 代码框架
1. `cargo new projectName1`使用包管理器创建一个新项目。创建成功后项目目录如下所示：  

   ```
   projectName1
   ├── Cargo.lock
   ├── Cargo.toml
   ├── src
   │   └── main.rs
   └── target
   ```
+ 其中，Cargo.toml中  

   ```c
   [package]
   name = "projectName1"
   version = "0.1.0"
   edition = "2021"

   [dependencies]
   ```
2. 在`toml`文件中加上某些依赖，就能在代码中使用相应函数


##### 5.2 示例代码分析
```rust
fn main()-> Result<(), Box<dyn Error>>{
    println!("Hello, world!");
    let args: Vec<String> = env::args().collect();

    if args.len() != 3 {
        println!("usage:");
        println!("sha1-cracker:<worklist.txt><sha1-hash>");
        return Ok(());
    }

    let hash_to_crack = args[2].trim();
    if hash_to_crack.len() != SHA1_HEX_STRING_LENGTH{
        return Err("sha1 hash is not valid".into());
    }

    // args[0]:程序路径；args[1]:程序的第一个参数
    // ?操作符用于传播错误
    let wordlist_file = File::open(&args[1])?;
    let reader = BufReader::new(&wordlist_file);

    // reader.lines()返回一个迭代器result<String, std::io::Error>
    for line in reader.lines(){
        // let line = line?.trim().to_string();
        // println!("{}",line);
        let line = line?;
        let common_passwd = line.trim();
        // 在toml中添加依赖，在这里使用
        if hash_to_crack == &hex::encode(sha1::Sha1::digest(common_passwd.as_bytes())){
            println!("password found: {}", &common_passwd);
            return Ok(());
        }
    }
    println!("password not found in wordlist");

    Ok(())
}
```
###### 5.2.1[函数返回值](https://course.rs/basic/result-error/result.html) `Result<T,E>`
 
```rust
// 枚举类型，其中T是泛型参数，存放方式为Ok(T)
// E表示发生错误时存入的错误值，存放方式为Err(E)
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```
> Q:🤷🏻 🤷 🤷我怎么获得变量类型和函数返回值类型？
> A1:可查询标准库或第三方库函数⛔
> A2:可借助VScode+rust-analyzer获得变量类型和函数返回类型✅
> A3:分析报错信息✅

```sh
error[E0308]: mismatched types
  --> src/main.rs:18:16
   |
11 | fn main() {
   |          - expected `()` because of default return type
...
18 |         return Ok(());
   |                ^^^^^^ expected `()`, found `Result<(), _>`
   |
   = note: expected unit type `()`
                   found enum `Result<(), _>`
```
###### 5.2.2 Box与dyn

```rust
// 截取部分示例代码
fn main()-> Result<(), Box<dyn Error>>{
  ...
}
```

> `struct Box<T, A = Global>`，其中A是allocator;
> a pointer type that uniquely owns a heap allocaton of type `T`
+ 是什么？
  + `Box`是一个智能**指针**，指向堆分配的`T`类型的值。
  + `Box<T>`将值装箱使它能在堆上分配，当箱子离开作用域时，1️⃣会调用析构函数，2️⃣并销毁内部对象、3️⃣释放堆上内存。
  + 拆箱/解引用：`*`
+ 为什么？
  + 数据大小不确定时，使用堆分配避免栈溢出
  + 某些类型大小不确定，如递归数据结构
  + trait对象  
❓ <font color="#0e49a5">这里的trait和dyn是啥</font>

###### 5.2.3 <font color="#0047a5">Trait对象与dyn</font>
+ 是什么？
  + `trait`类似于其它语言中的`interface`
  + `dyn`是一个关键字，
    + 使用`dyn`关键字，不在编译阶段静态确定，取而代之的是在运行阶段通过动态分发来调用`trait`方法
+ 静态分发与动态分发
  + 静态分发：在编译时确定方法调用的具体实现，使用泛型(❓<font color="#F14665">什么是泛型</font>)和`impl Trait`，不支持多态。
  + 动态分发：在运行时才确定，使用`dyn Trait`
  
```rust
// 这里举一个使用dyn T的例子
// 运行时根据“动物类型”动态决定“speak”方法
let animals: Vec<Box<dyn Animal>> = vec![Box::new(Dog), Box::new(Cat)];
for animal in animals {
    animal.speak();
}
```

###### 5.2.4 泛型参数
+ <font color="#F14665">泛型参数</font>
  + 在定义函数、结构体、枚举、方法等时，使用占位符表示任意类型
  + 使用trait约束来限制泛型参数的类型范围
```rust
// 泛型参数举例--结构体
struct Point<T> {
    x: T,
    y: T,
}
let int_point = Point { x: 5, y: 10 };

// 泛型参数举例--使用trait约束
// T: std::fmt::Display 表示T必须实现Display接口
fn print_value<T: std::fmt::Display>(value: T) {
    println!("{}", value);
}  
```
###### 5.2.5 RAII(资源获取时初始化)
```rust
// 截取部分示例代码
let wordlist_file = File::open(&args[1])?;
```

❓ 文件操作只有`open`，没有`close`  

> `RAII(resource acquisition is initialization)`资源获取时初始化
> 在rust中，变量不仅指代相应的内存，也拥有资源（如下面例子中的文件资源）。

+ 当超出作用域时，对象会自动调用析构函数，释放内存
  + 这里的文件句柄`wordlist_file`的作用域为`main`函数，当`main`函数返回时，文件会自动关闭

###### 5.2.6 Ok(())
+ statements-oriented language 声明式语言  
  + `return Ok(());`以“;”结尾   
+ expression-oriented language 表达式语言  
  + `Ok(())`没有标点符号  

#### 6. For:初学者
+ All this stuff is really interesting and produced by very smart people, but it prevents you from getting things done.
+ 命令式语言+函数式语言


##### Rust的内存安全机制

###### 所有权


###### 借用

###### 生命周期
> lifetime
> 编译器需要知道**引用的数据**在内存中存活的时间，以保证**引用**(🤷🏻<font color="#AA57FF">什么是引用</font>)在有效范围内是安全的  
> 如果编译器无法推断出引用的生命周期，则要求程序员显式提供生命周期注释  

+ 生命周期注释
  + 用法：`'`+`标识符`，如`'a`，通常放在函数签名（🤷🏼<font color=green>什么是函数签名</font>）或结构体的定义中


---

1. <font color="#AA57FF">引用-Reference</font>
> 指向另一个变量数据的指针，允许在不移动数据所有权的情况下访问或使用数据，从而实现共享和安全的数据访问
> 核心是借用变量，而不改变变量的所有权

+ 不可变引用`&`    
  + 只读访问数据，不能修改引用指向的数据
+ 可变引用`&mut`
    + 允许修改数据
    + 同一时间只能有一个可变引用，且不能与不可变引用同时存在
```rust
// 不可变引用和可变引用不能同时存在
fn main() {
    let mut s = String::from("hello");
    let r1 = &s;       // 不可变引用
    let r2 = &mut s;   // 编译错误：不可变引用和可变引用不能同时存在
    println!("{}, {}", r1, r2);
}
```
```rust
// 编译器使用 借用检查器borrow checker 防止出现悬垂引用
fn main() {
    let r;
    {
        let x = 5;
        r = &x; // 编译错误：x 的生命周期在此块结束时结束
    }
    println!("{}", r); // 如果编译通过，r 将指向无效的内存
}

```

+ 另外，Rust也支持函数传参引用、多线程引用


2. <font color=green>函数签名</font>
  + 一般来说，包括：函数名、参数列表、返回类型
  + 作用：
    + 描述函数接口，方便被调用
    + 编译器用来验证类型是否匹配，以保证函数调用的类型安全 （🤷<font color="#FF5733">什么是类型安全</font>）
  + Rust中还可能包括
    + 泛型参数
    + 生命周期注释

3. <font color="#FF5733">类型安全</font>
  + Rust是静态类型语言，在编译时会进行类型检查。同时支持类型推断，因此不需要显示标注每个变量的类型。
  + 但是，不允许隐式的类型转换，必须显式的进行。

---

###### 引用计数智能指针
> 生命周期注释增加了代码复杂性、降低了可读性
> 对一些共享（or not）、可变（or not）引用来说，使用生命周期注释相当麻烦
> 一种解决方案是：引用计数智能指针`std::rc::Rc`(`reference counting`)

```rust
use std::rc::Rc;

fn main() {
    let pointer = Rc::new(String::from("Hello, Rust!"));
    
    {
        // 这里调用Rc.clone()只会增加引用计数，而不需要深拷贝
        // 当引用计数为0时，才会清理数据
        let second_pointer = Rc::clone(&pointer); // 增加引用计数
        println!("{}", second_pointer); // 访问共享的 `String` 数据
    } // `second_pointer` 超出作用域，引用计数减少
    
    println!("{}", pointer); // 原始引用仍然有效
}
```
---
❓<font color="#9900f7">**引用计数** 天然的 存在**循环引用**的问题</font>

---

+ Rc--单线程场景下实现引用计数
```rust
use std::cell::{Refcell, RefMut};
use std::rc::Rc;
```

+ Arc--多线程场景下实现引用计数

###### 维护与更新
+ rustup 本地工具链
```shell
rustup self update
rustup update
```
+ rust fmt 代码格式化工具`cargo fmt`
+ clippy 检测可能导致错误的代码
```shell
rustup component add clippy
cargo clippy 
```
+ cargo update
```shell 
cargo update
```
