---
title: 轮播图滚动条的css布局理解
date: 2018-12-18 09:56:00
tags: css
categories: 前端
---

## 一、需求描述

做一个waymo的滚动条，在页面中显示两张图，一共4张图，无限滚动播放。

``` css
.car{
  width: 600px;
  height: 157px;
  margin: 100px auto;
  background-color: red;
  overflow: hidden;
}
ul>li{
  list-style: none;
  float: left;
  width: 300px;
  height: 157px;
}
```

## 二、问题描述

1. 全部图片放不进容器里，因为父容器只有600px，4张图片1200px，后面的图只能被挤到第二排。加动画效果时，等图片左移，第一排的空够300px放一张图时，图片才放到第一排。动画效果十分不理想。

2. 用动画控制图片marginLeft，来制造左移效果。图片第四张切到第一张时转换十分生硬，不连贯。

## 三、问题解决

1. 解决方法如下：

在li的父级容器ul里设置好足够的空间，在ul的父级容器里规定overflow为hidden，因此，li能放在一排，并且显示部分不会超出car的规定范围。（隐藏溢出）

``` css
.car{
  width: 600px;
  height: 157px;
  margin: 100px auto;
  background-color: red;
  overflow: hidden;
}
ul{
  width: 1800px;
  height: 157px;
}
ul>li{
  list-style: none;
  float: left;
  width: 300px;
  height: 157px;
}
```
2. 增加两张图，将1、2两图添至图片列表末尾，所以一共有6张图：123412。

这样做的好处是：图1切到末尾的图1时，因为图片相同，所以切换效果很连贯。


#### 附录: [overflow:hidden](https://blog.csdn.net/qq_41638795/article/details/83304388) 作用：

1.隐藏溢出

2.清除浮动

3.解决外边距塌陷