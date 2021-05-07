---
title: RN Flatlist的onEndReached多次触发问题解决
date: 2019-04-26 20:25:08
tags: 
  - JavaScript 
  - React Native
categories: 调试测试
---

## 一、问题

RN项目里使用Flatlist组件，上拉刷新item过多时，出现跳屏、闪屏、空白屏等问题。

## 二、原因

先在render函数里log了一下，发现没有re-render，判断不是网络请求或页面内组件数据变动导致的重复渲染；

然后判断是Flatlist自己的触底监听机制有问题；

最后查到是因为最外层父View没有设置固定height或只设置{flex：1}属性，导致onEndReached不能正确监听事件。

## 三、解决

1. 给最外层父组件一个固定高度{height：‘100%’}；

2. 设置onEndReachedThreshold={0.01}，确保滑动到距离底部最后0.01时再调动加载功能即可。

补充：

发现scrollview组件出现在部分机型上滑动无效（例如华为p20）的问题时，采用上述方法能够解决问题。