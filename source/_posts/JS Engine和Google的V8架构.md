---
title: JS Engine和Google的V8架构
date: 2022-02-08 11:55:08
tags: 
  - 浏览器Chrome
  - 基础知识
categories: 前端
---

#### 一、JS runtime environment(运行时)
JS代码需要在相应的JS runtime environment中运行，常见的浏览器和node.js都有JS runtime environment：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1644291251857-690d83fe-952f-4ca1-8cd5-a91696924155.png#clientId=uae2804b5-d62c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=673&id=uc47c18bc&margin=%5Bobject%20Object%5D&name=image.png&originHeight=673&originWidth=879&originalType=binary&ratio=1&rotation=0&showTitle=false&size=466401&status=done&style=none&taskId=u54bd991b-283d-4b38-9d25-684005ae96f&title=&width=879)
浏览器和node.js的runtime environment不同，其内置的api也不同。
JS runtime environment里包括JS Engine，event loop，api，callback queue和microtask queue。
#### 二、JS Engine
JS Engine负责处理JS代码，是JS运行的关键。它接收我们编写好的JS代码作为input，然后执行以下三步：

1. Parsing(解析)：负责把一行行代码解析成AST(Abstract Syntax Tree, 抽象语法树)
1. Compile(编译)：同时对AST进行解释和编译优化，转换成机器码
1. Execution(执行)：接收low level的机器码并快速执行

![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1644291209862-a17491fc-349e-49ef-9ebf-7f77344dd767.png#clientId=uae2804b5-d62c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=510&id=u061bf191&margin=%5Bobject%20Object%5D&name=image.png&originHeight=510&originWidth=595&originalType=binary&ratio=1&rotation=0&showTitle=false&size=95922&status=done&style=none&taskId=ubaad0005-318c-4d53-9013-619280a19cb&title=&width=595)
上图为Google官网的V8架构图。
#### 三、JIT compilation(Just in time，即时编译)
JIT compilation是JS特有的编译方式，指解释和编译同步运行。其中，Compiler有很多优化器。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1644291162414-5e1a2f9e-6e7c-474c-874f-2892273ade69.png#clientId=uae2804b5-d62c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=631&id=douAI&margin=%5Bobject%20Object%5D&name=image.png&originHeight=631&originWidth=912&originalType=binary&ratio=1&rotation=0&showTitle=false&size=575099&status=done&style=none&taskId=u4b3b1e4c-40a2-4ff3-b16f-5cd8eb7c529&title=&width=912)
Interpreter和Compiler之后形成的Bytecode(机器码)在执行的过程中离不开JS Engine的两个重要组件：

- Memory Heap：用于内存分配管理，由garbage collector进行垃圾回收处理
- Callback Stack：用来存储待执行的task

参考：

1. [JS Engine EXPOSED 🔥 Google's V8 Architecture 🚀 | Namaste JavaScript Ep. 16](https://www.youtube.com/watch?v=2WJL19wDH68)
