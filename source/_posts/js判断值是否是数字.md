---
title: js判断值是否是数字
date: 2018-11-29 22:30:08
tags: 基础知识
categories: 前端
---

1. isNaN（）方法
2. 正则表达式
var re = /^[0-9]+.?[0-9]*$/; //判断字符串是否为数字 //判断正整数 /^[1-9]+[0-9]*]*$/
3. 利用parseFloat的返回值
isNaN(inputData)不能判断空串或一个空格；
如果是一个空串或是一个空格，而isNaN是做为数字0进行处理的，而parseInt与parseFloat是返回一个错误消息，这个isNaN检查不严密而导致的。
parseFloat(inputData).toString() == "NaN"