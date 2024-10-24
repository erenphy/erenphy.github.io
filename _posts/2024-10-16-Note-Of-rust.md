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

#### Rust编程语言

|编程语言|问题|
|:--:|:--:|
| C,C++|不够安全|
|python|慢；弱类型导致难以编写大型程序|
|Java|依赖较长的运行时间|

#### Awesome Rust
+ 




