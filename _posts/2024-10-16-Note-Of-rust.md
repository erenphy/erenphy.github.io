---
layout:     post
title:      black-hat-rust
subtitle:   阅读笔记
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
#### 攻击阶段
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




#### 攻击者简介
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

#### Rust与其他语言的区别

|编程语言|问题|
|:--:|:--:|
| C,C++|不够安全|
|python|慢；弱类型导致难以编写大型程序|
|Java|依赖较长的运行时间|

#### Awesome Rust
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
+  


