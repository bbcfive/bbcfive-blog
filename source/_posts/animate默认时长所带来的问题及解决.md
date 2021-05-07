---
title: jQuery.animate默认时长所带来的问题及解决
date: 2018-12-19 17:23:00
tags: 
  - JavaScript
  - jQuery
  - 基础知识
categories: 前端
---
## 一、需求描述
做一个进度条长度逐渐减少的动画，当进度条长度小于等于0时，关闭动画，并弹出透明底板显示新提示。

## 二、问题描述
初始代码如下：
``` js
//设置进度条初始长度
var progressLength = 180;    
//设置一个定时器    
var timer = setInterval(function () {
  //开始游戏后进度条逐渐消失
  progressLength -= 10;
  $(".progress").animate({
    width: progressLength
  });
  console.log("hh");
  //如果进度条走到尽头
  if (progressLength <= 0) {
    clearInterval(timer);
    $(".over").fadeIn(100);
  }                        
}, 50);
```
遇到问题：

进度条递减的速度快于动画速度，导致动画还没执行完，progressLength就已经≤0，底板就弹出来了。

## 三、问题解决
首先，导致问题的原因是：

animate动画执行有默认时长，即1000（1s），就是第二个参数。

而先前设置的定时器执行时长是50，导致两厢不匹配，即animate函数域里外不是一个速度。

解决方法：（消灭时间差）

1.设置animate第二个参数，使执行时长等于外面定时器的执行时长；

<font color="#FF4500">（不推荐，因为两个函数之间总有执行上的时间差）</font>

``` js
//设置进度条初始长度
var progressLength = 180;    
//设置一个定时器    
var timer = setInterval(function () {
  //开始游戏后进度条逐渐消失
  progressLength -= 10;
  $(".progress").animate({
    width: progressLength
  },50);
  console.log("hh");
  //如果进度条走到尽头
  if (progressLength <= 0) {
    clearInterval(timer);
    $(".over").fadeIn(100);
  }                        
}, 50);
```

2.改animate为css，这样就变成静态函数，没有时间差了

``` js
//设置进度条初始长度
var progressLength = 180;    
//设置一个定时器    
var timer = setInterval(function () {
  //开始游戏后进度条逐渐消失
  progressLength -= 10;
  $(".progress").css({
    width: progressLength
  });
  console.log("hh");
  //如果进度条走到尽头
  if (progressLength <= 0) {
    clearInterval(timer);
    $(".over").fadeIn(100);
  }                        
}, 50);
```