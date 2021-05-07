---
title: 多版本python如何切换
date: 2019-01-22 11:48:08
tags: 基础知识
categories: 后端
---

## 一、在命令行中

通过py -x

![avatar](https://img2018.cnblogs.com/blog/1549437/201901/1549437-20190122114500383-1266605908.png)

## 二、在py文件中

头部字段添加

#!python2 或 #!python3

即可调用相应版本解释器

命令行调用python：py helloworld.py