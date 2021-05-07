---
title: svg整体缩放至指定大小
date: 2019-07-29 22:25:01
tags:  
  - JavaScript
  - svg
categories: 前端
---
## 一、问题
svg画面跑在分辨率低的电脑上，导致不能完全显示。

## 二、要求
svg要能够根据电脑的屏幕大小自动缩放至适配电脑的尺寸。

## 三、实现
1. 获取本机窗口高度、宽度
> let clientWidth = document.documentElement.clientWidth,
    clientHeight = document.documentElement.clientHeight;    

2. 获取缩放比例（svg大小设为x，y）
> let widthScale = clientWidth / x, heightScale = clientHeight / y;
 

3. svg整体缩放
svg的特点是：缩放后，元素的中心坐标也会跟着变化缩放，所以元素的位置会偏移。为防止这种偏移，对svg不仅仅要进行<font color="#FF4500">scale（缩放）</font>，还要进行<font color="#FF4500">translate（中心平移）</font>。

测试后发现缩放倍数x与平移坐标y的关系成一次函数：y = 0.5x - 0.5；

于是得出公式：
``` js
svg.setAttribute("transform", `
    translate(${ x * (widthScale - 1) / 2}, ${ y * (heightScale - 1) / 2})
    scale(${widthScale}, ${heightScale})
`);
```
此时，svg图像会铺满电脑屏幕。

## 四、功能延展
有时，svg图像不仅仅是为了适配屏幕，而是可以动态放大缩小； 
此时，先给定一个原始大小：height，width，方便以此为基准进行缩放
> barChart.setAttribute('width', barChartWidth); 
  barChart.setAttribute('height', barChartHeight);

再给定自由设置svg大小的变量：
> let svgW = 60, svgH = 20;

算出缩放比例：
> let widthScale = svgW / barChartWidth, heightScale = svgH / barChartHeight;

进行缩放：
``` js
// 缩放要注意的是
// 1.把新生成的svg中心坐标重新定位到页面左上角
// 2.大小按比例同时缩放
barChart.setAttribute("transform", `
    translate(${ barChartWidth * (widthScale - 1) / 2}, ${ barChartHeight * (heightScale - 1) / 2})
    scale(${widthScale}, ${heightScale})
`);
```

最终效果：
1. 设svgW = 60, svgH = 20; （找得到svg在哪吗？）
![avatar](https://img2018.cnblogs.com/blog/1549437/201907/1549437-20190729222259064-766875909.png)

2. 设svgW = 180, svgH = 60;
![avatar](https://img2018.cnblogs.com/blog/1549437/201907/1549437-20190729222405898-737093672.png)

3. 设svgW = 600, svgH = 200;
![avatar](https://img2018.cnblogs.com/blog/1549437/201907/1549437-20190729222431594-349553552.png)

大功告成~