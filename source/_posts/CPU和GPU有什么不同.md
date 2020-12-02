---
title: CPU和GPU的区别是什么？
date: 2020-11-09 15:03:20
tags: 
  - 基础原理
categories: 计算机原理
---

## CPU 和 GPU 的区别是什么？
本文是对[1.2CPU和GPU的设计区别](https://www.cnblogs.com/biglucky/p/4223565.html)一文的阅读总结。

一、 为什么产生区别
  
  CPU 和 GPU 的不同，是因为其设计目标的不同：
  - CPU：需要很强的通用性来处理各种不同的数据类型，同时又要逻辑判断，又会引入大量的分支跳转和中断处理，因此CPU内部结构异常复杂。
  - GPU：面对类型高度统一、相互无依赖的大规模数据和不需要被打断从纯计算环境。
     
    因此两者呈现出不同的架构：
    ![avatar](https://pic1.zhimg.com/80/918367f36e34c18dc1f92bd16760dae1_720w.jpg?source=1940ef5c)

二、架构区别
  - CPU：大量cache空间、复杂的控制逻辑、诸多优化电路。
  - GPU：数量众多的计算单元、超长的流水线、非常简单的控制逻辑、无cache。
  ![avatar](https://pic3.zhimg.com/80/894e6d2f20921e7c8be985bbb0dac5d5_720w.jpg?source=1940ef5c)
  注：
  1. 多寄存器可以支持非常多的Thread,thread需要用到register,thread数目大，register也必须得跟着很大才行。
  2. SIMD(Single Instruction Multiple Data)架构：单指令多数据流，以同步方式，在同一时间内执行同一条指令。

三、总结
  CPU有强大的ALU（算术运算单元），能够处理复杂的计算问题。GPU是基于大的吞吐量设计，适用于计算密集型程序和易于并行程序的运行。