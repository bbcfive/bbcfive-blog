---
title: delegate与hover和blur冲突问题及解决方法
date: 2018-12-22 10:31:00
tags: 
  - JavaScript
  - jQuery
  - 基础知识
categories: 前端
---

## 一、冲突

hover和blur都是含有两个函数参数的方法，分别表示事件的两种对立状态的相应方法。

delegate用于处理事件委托等场景，只能传一个函数参数。

冲突：delegate无法完整传入hover和blur的两个函数参数。

## 二、解决方法

回归hover和blur的本源：用mouseenter和mouseleave替代hover和blur的两种状态就行了。

``` js
//监听鼠标移入时歌曲行图标的出现
$(".music_list").delegate(".list_music", "mouseenter", function () {
  //显示图片
  $(this).find(".list_menu").addClass("show_list");
  //隐藏时长showList
  $(this).find(".list_time").addClass("show");
});
//监听鼠标移出时
$(".music_list").delegate(".list_music", "mouseleave", function() {
  //隐藏图片
  $(this).find(".list_menu").removeClass("show_list");
  //显示时长
  $(this).find(".list_time").removeClass("show");
});
```