---
layout: post
title: "并行加速"
date: 2025-03-24
tags: [多核并行，SIMD，单核CPU，CPU加速，GPU加速，向量化硬件]
---

### 写在前面
+ 程序性能优化的几个思路：
  + 流水线（单核）
  + 多线程（单核）
  + SIMD指令集(单核)
  + 单核 &rightarrow; 多核

### 多线程
#### OpenMP多线程并行计算
+ `#pragma omp parallel{}`创建多个线程，每个线程执行`{}`内的代码

### SIMD指令集-单指令多数据

#### 自动向量化（编译器）
#### 手动向量化（程序员）
+ 使用编译器指令`#pragma`
  + `#pragma`向编译器提供特定的控制信息
    + 优化编译`#prama optimize`,`#pragma vector`
    + 控制警告`#pragma warning`
    + 并行计算`#pragma omp parallel`
    + 控制内存对齐`#prama pack`

1. 启动SIMD指令集
   1. `AVX`(Aavanced vector extensions,高级向量扩展指令集)
   2. `SSE`(Streaming SIMD extensions,)
2. `Intel`编译器向量化`#pragma vector always`
   1. 强制向量化
