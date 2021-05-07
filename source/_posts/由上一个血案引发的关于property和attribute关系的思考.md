---
title: 由上一个血案引发的关于property和attribute关系的思考
date: 2019-04-02 10:16:10
tags: 基础原理
categories: 前端
---

boss说，学习要刨根问底。

好的，开刨。

## 一、property和attribute在英语里有什么区别
![avatar](https://img2018.cnblogs.com/blog/1549437/201904/1549437-20190402095648825-1179073677.png)

看似没有区别。但其实大神说：

property是 物体本身自带属性，不能改变的（一旦改了就是另外一个东西了） =》 化学变化

attribute，由于 attribute还可以做动词，表示赋予。。。特性，属于人为赋予的可改变的属性。 =》 物理变化

比如，你的头发，可以人为拉直、弯曲，但不管怎么样，都是你的头发，这叫做头发的attribute。

但是头发的弹性、硬度，这些没办法改变，改了就不是头发了，这是property.

## 二、property和attribute在编程语言里有什么区别

HTML里
``` html
// gameid和id都是attribute节点
// id同时又可以通过property来访问和修改
<div class="box" id="box" gameid="880">hello</div>
// areaid仅仅是property
elem.areaid = 900;
```

RN里
``` html
// gameid和id都是props
<View id="box" gameid="880">hello</View>
// areaid仅仅是attribute
elem.areaid = 900;
```

结论：不同编程语言里的property和attribute区别不同。。。

更深层次的区别：

还没发现。。。。。。。。。