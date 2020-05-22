---
title: JSON.parse()与JSON.stringify()的区别
date: 2018-12-07 15:31:12
tags: 
  - JavaScript
  - 基础知识
categories: 前端  
---


#### JSON.parse()【从一个字符串中解析出json对象】

例子：

//定义一个字符串

var data='{"name":"goatling"}'

//解析对象​

​JSON.parse(data)

结果是：

​name:"goatling"

#### JSON.stringify()【从一个对象中解析出字符串】

var data={name:'goatling'}

JSON.stringify(data)

结果是：

'{"name":"goatling"}'