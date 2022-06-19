---
title: 浏览器的 5 种 Observer
date: 2022-02-15 11:37:08
tags: 
  - 浏览器Chrome
  - 基础知识
categories: 前端
---

监听用户的交互行为，我们会用 addEventListener 来监听 click、mousedown、keydown、input 等事件，但对于元素的变化、performance 的记录、浏览器干预行为这些不是用户交互的事件就要用 XxxObserver 的 api 了。
浏览器提供了这 5 种 Observer：

- IntersectionObserver：监听元素可见性变化，常用来做元素显示的数据采集、图片的懒加载
- MutationObserver：监听元素属性和子节点变化，比如可以用来做去不掉的水印
- ResizeObserver：监听元素大小变化

还有两个与元素无关的：

- PerformanceObserver：监听 performance 记录的行为，来上报数据
- ReportingObserver：监听过时的 api、浏览器的一些干预行为的报告，可以让我们更全面的了解网页 app 的运行情况

这些 api 相比 addEventListener 添加的交互事件来说用的比较少，但是在特定场景下都是很有用的。

原文：[https://mp.weixin.qq.com/s/-fLZe164aWAaPJ55iZ_m2w](https://mp.weixin.qq.com/s/-fLZe164aWAaPJ55iZ_m2w)
