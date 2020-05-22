---
title: RN canvas缩放处理
date: 2019-04-16 15:48:10
tags: 
  - JavaScript 
  - React Native
  - canvas
categories: 移动端
---

## 一、关于canvas缩放

canvas图像缩放处理有两种思路：

ctx.scale()，对整个canvas进行重绘，会导致每次缩放都重新加载，影响体验效果
在canvas外包层view，直接对外层的view进行缩放

## 二、view触摸事件

view组件借助RN自带的手势响应系统，已经有完善的触摸事件处理体系。

RN触摸事件处理详解：[https://www.race604.com/react-native-touch-event/](https://www.race604.com/react-native-touch-event/)

其中，PanResponder是一个封装好的用于处理多点触摸交互的手势系统。

基本用法：[https://reactnative.cn/docs/panresponder/#docsNav](https://reactnative.cn/docs/panresponder/#docsNav)

## 三、关于缩放

A pure JavaScript RN component that makes ANY views transformable using gestures like pinch, double tap or pull.：[react-native-view-transformer](https://github.com/ldn0x7dc/react-native-view-transformer)

用轮子封装自定义组件即可：

``` html
Usage

import ViewTransformer from 'react-native-view-transformer';
...
render() {
  return (
      <ViewTransformer>
      //ANY views
    </ViewTransformer>
  );
}
```

一个及其有效的学习方式：研究别人轮子的实现方式，既有助于深入理解RN，同时还能激发想象力。

血的教训：

当你搜索“RN 手势识别”、“panresponder 缩放”等等，发现超过1个小时还没有解决问题，那么请直接搜索“RN gesture recognition”、“panresponder zoom”，会有意想不到的收获。