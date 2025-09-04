---
layout: post
title: "Rust-Helloworld"
date: 2025-08-27
tags: [编程语言，CPP，Rust]
---


## Setup

|      |      |   |
|:----:|:--:  |:--|
|`rustc` |`clang` |rust编译器|
|`cargo` |`make`  |管理项目、依赖、构建、运行、测试、发布|
|`rustup`|`update-alternatives`|安装、更新工具链和版本|

#### 安装rustup
> 借助rustup管理工具链  
> 可执行`rustup update`进行升级更新  
+ 安装方法
  + windows [下载地址](https://www.rust-lang.org/zh-CN/tools/install) 
  + 类unix安装命令
`curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`  
    + 但是老报错👀，可能压缩包太大了，网络状况堪忧，推荐下一种方法  
  
  + [官方链接](https://forge.rust-lang.org/infra/other-installation-methods.html)找到适合自己操作系统的压缩包下载、解压，然后运行`sudo ./install.sh`进行安装  
    + 即可获得编译器`rustc`、包管理器`cargo`

+ 验证🥂🥂🥂  

```bash
$ rustc --version
rustc 1.82.0 (f6e511eec 2024-10-15)
$ cargo --version
cargo 1.82.0 (8f40fc59f 2024-08-21)
```

#### cargo 使用准备
+ **换国内源**
  + 在`/home/.cargo/config.toml`中添加
  
```toml
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"
```

+ 添加环境变量
  + `cargo install rustfmt`执行后，有一个warning：`warning: be sure to add `/home/lxh/.cargo/bin` to your PATH to be able to run the installed binaries`提示需要将`bin`加入环境变量
    + 查看当前环境变量`echo $PATH`
  + 方法1：`export PATH=/home/lxh/.cargo/bin:$PATH`
    + 缺点：一次性修改
  + 方法2：修改`~/.bashrc`文件，并终端执行`$ source ~/.bashrc`立即生效，且永久有效  
  + 方法3：修改`/etc/profile`，同2

#### 配置编辑器
+ 在vscode中装一个`rust-analyzer`插件，支持命令补全、代码检查

### Cargo与Rust项目
#### 项目管理
```bash 
$ cargo -h
Rust's package manager

Usage: cargo [OPTIONS] [COMMAND]
       cargo [OPTIONS] -Zscript <MANIFEST_RS> [ARGS]...

Options:
  -V, --version             Print version info and exit
      --list                List installed commands
      --explain <CODE>      Provide a detailed explanation of a rustc error message
  -v, --verbose...          Use verbose output (-vv very verbose/build.rs output)
  -q, --quiet               Do not print cargo log messages
      --color <WHEN>        Coloring: auto, always, never
  -C <DIRECTORY>            Change to DIRECTORY before doing anything (nightly-only)
      --locked              Assert that `Cargo.lock` will remain unchanged
      --offline             Run without accessing the network
      --frozen              Equivalent to specifying both --locked and --offline
      --config <KEY=VALUE>  Override a configuration value
  -Z <FLAG>                 Unstable (nightly-only) flags to Cargo, see 'cargo -Z help' for
                            details
  -h, --help                Print help

Commands:
    build, b    Compile the current package
    check, c    Analyze the current package and report errors, but don't build object files
    clean       Remove the target directory
    doc, d      Build this package's and its dependencies' documentation
    new         Create a new cargo package
    init        Create a new cargo package in an existing directory
    add         Add dependencies to a manifest file
    remove      Remove dependencies from a manifest file
    run, r      Run a binary or example of the local package
    test, t     Run the tests
    bench       Run the benchmarks
    update      Update dependencies listed in Cargo.lock
    search      Search registry for crates
    publish     Package and upload this package to the registry
    install     Install a Rust binary
    uninstall   Uninstall a Rust binary
    ...         See all commands with --list

See 'cargo help <command>' for more information on a specific command.
```

##### 0. init

```bash
sha1$ cargo init
    Creating binary (application) package
note: see more `Cargo.toml` keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

sha1$ tree 
.
├── Cargo.toml
└── src
    └── main.rs

1 directory, 2 files

sha1$ cat Cargo.toml 
[package]
name = "sha1"
version = "0.1.0"
edition = "2021"

[dependencies]
```

+ `.toml`配置文件
  + 类似于`Node.js`的`package.json`
  + `[package]`：项目名、版本、作者等元信息。
    + `edition`
      + 2015: Rust 1.0
      + 2018: Rust 1.31
      + 2021: Rust 1.56
  + `[dependencies]`：依赖库，如 `sha1 = "0.6"`
+ `crate`
  + 最小编译单元
  + `main.rs`
    + 可执行程序入口，`fn main()`
    + `binary crate`
  + `lib.rs`
    + `library crate`

##### 1. run

```rust
fn main() {
    println!("Hello, world!");
}
```

```bash
sha1/src$ cargo run main.rs 
   Compiling sha1 v0.1.0 (/sha1)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 5.66s
    Running `sha1 main.rs`
Hello, world!
```
+ `Println!`
  + [Rust宏是什么](https://doc.rust-lang.org/book/ch19-06-macros.html)   

```bash
# Cargo stores the result of the build in the target/debug directory.
/sha1$ tree
.
├── Cargo.lock
├── Cargo.toml
├── src
│   └── main.rs
└── target
    ├── CACHEDIR.TAG
    └── debug
        ├── build
        ├── deps
        │   ├── sha1-6468a928cc7d1c4c
        │   └── sha1-6468a928cc7d1c4c.d
        ├── examples
        ├── incremental
        │   └── sha1-3gwi201aeuma9
        │       ├── ...
        ├── sha1
        └── sha1.d

9 directories, 18 files
```


### Our demo

```rust
use sha1::{Sha1, Digest};
use hex;

fn main() {
    let password = "hello";
    let hash = Sha1::digest(password.as_bytes());
    println!("Password: {}", password);
    println!("SHA1 hash (hex): {}", hex::encode(hash));
}
```

#### 1. use
> 用于引入模块、结构体、函数、`trait` 或者常量到当前作用域  

+ 引入标准库

```rust
use std::collections::HashMap;//数据结构
use std::io::{self, Write，Read};//IO操作、引用多个
use std::fs::*;//文件操作、引用整个模块
use std::collections::HashMap as Map;//取别名
```
+ 引入第三方库
  + 需要在`toml`中先添加依赖  
```toml
[dependencies]
regex = "1.9"
```
+ 引入trait
  + 相对复杂，结合例子来看：`use sha1::{Sha1, Digest};`,`let hash = Sha1::digest(password.as_bytes());`
  + `Sha1`是一个结构体
  + `Digest`是`sha1`库中的一个`trait`
    + `fn digest()`是其中的一个关联函数/静态方法
      + 关联函数：没有`self`参数，只能通过类型名调用

> `Sha1`：引入类型，告诉编译器我要用`SHA-1`算法的实现  
> `Digest`：引入`trait`，编译器才能知道`Sha1`拥有`digest()`这个方法

```rust
pub trait Digest {
    fn new() -> Self;
    fn update(&mut self, data: impl AsRef<[u8]>);
    fn finalize(self) -> Output<Self>;
    fn digest(data: impl AsRef<[u8]>) -> Output<Self>; // 关联函数
}
```

#### 2. crate版本
+ `sha1 = 0.6`旧版本报错
```bash
sha1/src$ cargo build
    Updating `ustc` index
     Locking 3 packages to latest compatible versions
      Adding hex v0.4.3
      Adding sha1 v0.6.1 (latest: v0.10.6)
      Adding sha1_smol v1.0.1
  Downloaded sha1 v0.6.1 (registry `ustc`)
  Downloaded sha1_smol v1.0.1 (registry `ustc`)
  Downloaded 2 crates (12.7 KB) in 8.23s
   Compiling sha1_smol v1.0.1
   Compiling hex v0.4.3
   Compiling sha1 v0.6.1
   Compiling sha1 v0.1.0 (/home/.../sha1)
warning: unused import: `Digest`
 --> src/main.rs:1:18
  |
1 | use sha1::{Sha1, Digest};
  |                  ^^^^^^
  |
  = note: `#[warn(unused_imports)]` on by default

error[E0308]: mismatched types
   --> src/main.rs:6:29
    |
6   |     let hash = Sha1::digest(password.as_bytes());
    |                ------------ ^^^^^^^^^^^^^^^^^^^ expected `&Sha1`, found `&[u8]`
    |                |
    |                arguments to this function are incorrect
    |
    = note: expected reference `&Sha1`
               found reference `&[u8]`
```
+ `sha1 = 0.10`最新版本
```bash
sha1/src$ cargo build
    Updating `ustc` index
     Locking 10 packages to latest compatible versions
      Adding block-buffer v0.10.4
      Adding cfg-if v1.0.3
      Adding cpufeatures v0.2.17
      Adding crypto-common v0.1.6
      Adding digest v0.10.7
      Adding generic-array v0.14.7 (latest: v1.2.0)
      Adding libc v0.2.175
    Updating sha1 v0.6.1 -> v0.10.6
      Adding typenum v1.18.0
      Adding version_check v0.9.5
  Downloaded block-buffer v0.10.4 (registry `ustc`)
```

## 1. Sha1哈希算法
+ SHA-1 结构与输出
  + 不可逆、单向的加密摘要函数—原则上不能“反向”还原输入
  + 输出长度为160位（20字节），常用`hex` 表示为 `40` 位十六进制字符串
+ 安全性缺陷与弃用状态
  + 已知 SHA-1 存在碰撞攻击风险，被认为在安全上“已废弃”
+ 破解方法
  + **暴力猜测+ 哈希比较**
    + 对 `wordlist` 中每个候选做 SHA-1，然后比对。
    + 若`wordlist`不够，尝试所有可能的字符组合
  + `Rainbow table` 查表
  + `Key stretching`提升计算成本
## 2. Rust增量开发
>步骤提示：参数解析、字典加载（wordlist）、计算与对比、输出结果
### 程序入口与错误处理
+ 程序入口 `fn main()`
+ 返回值与错误处理 `Result<(), Box<dyn Error>>`
+ 使用语法糖`?`处理错误
### 命令行参数处理
+ `env::args().collect::<Vec<String>>()`
```rust
//std::env imports the module env from the standard library
//env::args()calls the args function from this module and returns an iterator which can be “collected” into a Vec<String> , a Vector of String objects. A Vector is an array type that can be resized.
use std::env;
```
+ 命令行参数数量检查
+ 常量检查：SHA-1哈希字符串是否符合长度
  + `const SHA1_HEX_STRING_LENGTH: usize = 40;`

### 文件读取与缓冲区
+ `std::fs::File::open`
+ `BufReader`提高读取性能
+ 逐行读取
### 字符串、字节处理
+ `String` vs `&str`
+ `.trim()`去除换行和空白
### 集合与迭代器
### 控制流
[分支结构/循环结构](https://doc.rust-lang.org/book/ch03-05-control-flow.html)
#### return
#### Ok(())

## 3. 组合子——集合与迭代器
### 集合
> Rust中常见的集合有哪些  
+ `a[]`数组
+ `vec![]`动态数组
+ `HashMap.new()`映射
### 迭代器Iterator
> An Iterator is an object that enables developers to traverse collections.

#### Owned vs Borrowed Iterator
```rust
fn vector() {
let v = vec![
    1, 2, 3,];
// .into_iter(): 所有权
for x in v.into_iter() {  
  println!("{}", x);
}
// you can't longer use v

fn hashmap() {
  let mut h = HashMap::new();
  h.insert(String::from("Hello"), String::from("World"));
  // .iter(): 借用
  for (key, value) in h.iter() {
    println!("{}: {}", key, value);
  }
}
}
```
#### 迭代器使用

+ `for .. in ..`循环
+ `for_each()`函数
+ `.collect()`将迭代器转换为集合

#### 举个栗子1：Collection
```rust
fn main() {
    let nums = 1..=10; // 迭代器: 1 到 10

    let total: i32 = nums.clone().sum();
    println!("sum() => {}", total); // 55

    let first_over_5 = nums.clone().find(|&x| x > 5);
    println!("find() => {:?}", first_over_5); // Some(6)

    let evens: Vec<_> = nums.clone().filter(|x| x % 2 == 0).collect();
    println!("collect() => {:?}", evens); // [2, 4, 6, 8, 10]
}

```
#### 举个栗子2：组合子

```rust
fn main() {
    let nums = 1..=10;

    let total: i32 = nums
        .filter(|x| x % 2 == 0)
        .filter(|x| *x > 5)
        .sum();

    println!("结果: {}", total); // 结果: 24  (6 + 8 + 10)
}
```

### 组合子Combinator
+ `.filter()`
+ `.inspect`
```rust
fn inspect() {
let v = vec![-1, 2, -3, 4, 5].into_iter();
let _positive_numbers: Vec<i32> = v
      //检查迭代的每个值
      .inspect(|x| println!("Before filter: {}", x))
      .filter(|x: &i32| x.is_positive())
      .inspect(|x| println!("After filter: {}", x))
      .collect();
}
```

```rust
src$ cargo run 
   Compiling myhello v0.1.0 (/home/rust/myhello)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 2.67s
     Running `/home/rust/myhello/target/debug/myhello`
Before filter: -1
Before filter: 2
After filter: 2
Before filter: -3
Before filter: 4
After filter: 4
Before filter: 5
After filter: 5
```
+ `.map()`将元素进行类型转换  
+ `.filter_map()`：相当于`.filter`和`.map`的组合，可用于处理`Option`类型数据  
+ `.chain()`：合并两个`iterators` 

```rust
fn main() {
let a = vec!["1","2","-1","4","-4","100","invalid","Not a number"," ",];
let _only_positive_numbers: Vec<i64> = a
.into_iter()
//.inspect(|x| println!("before filter_map: {}", x))
.filter_map(|x| x.parse::<i64>().ok())
//.inspect(|x| println!("After filter_map: {}", x))
.filter(|x| x > &0)
//.inspect(|x| println!("After filter: {}", x))
.collect();
}
```
