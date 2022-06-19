---
title: JS Engine和Google的V8架构
date: 2022-03-03 15:38:08
tags: 
  - Javascript
  - 基础知识
categories: 前端
---

![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1644908948518-78a8352e-aa70-41e9-a7c5-11b80891a698.png#clientId=ua6ca19fe-c102-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=669&id=u02592baf&margin=%5Bobject%20Object%5D&name=image.png&originHeight=669&originWidth=880&originalType=binary&ratio=1&rotation=0&showTitle=false&size=331015&status=done&style=none&taskId=u45968dc8-de8a-4aac-9832-fc04f669bb3&title=&width=880)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1646294301654-8f3d1064-aa76-4413-9402-0c5322c3ecaf.png#clientId=u5cbb3464-e2ea-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=817&id=ub4c77b19&margin=%5Bobject%20Object%5D&name=image.png&originHeight=817&originWidth=801&originalType=binary&ratio=1&rotation=0&showTitle=false&size=211454&status=done&style=none&taskId=ub370ca26-0c6e-40ee-b06d-5192e1658d2&title=&width=801)
用async：引入外部的脚本文件，文件间关联性小
用defer：引入的脚本文件相互依赖，需要维持执行顺序
