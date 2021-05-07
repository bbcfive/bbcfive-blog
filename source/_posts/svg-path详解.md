---
title: svg-path详解
date: 2018-12-03 22:00:08
tags: 
  - JavaScript 
  - svg
categories: 前端
---


## 一、svg的 <path>命令
M = moveto  <font color="#FF4500">//   m 50 20   =》  以（50,20）位置为起始点</font>
L = lineto   <font color="#FF4500">// m 50 20 l 20 50 =》从（50,20）到（20,50）作直线</font>
H = horizontal lineto  <font color="#FF4500">//  m 50 20 h 50  =》 从（50,20）到（50,50）绘制一条平行线</font>
V = vertical lineto  <font color="#FF4500">//  m 50 20 v 50  =》 从（50,20）到（100,20）绘制一条垂直线</font>
C = curveto 
S = smooth curveto
Q = quadratic Bézier curve
T = smooth quadratic Bézier curveto
A = elliptical Arc
Z = closepath <font color="#FF4500"> //  结束路径，回到原点</font>

<font color="#FF4500">注意：以上所有命令均允许小写字母。大写表示绝对定位，小写表示相对定位。</font>

## 二、svg的 <path>语法
1. d 引出路径
``` xml
<path d="M10 10 H 90 V 90 M100 100 H 180 V 180 H 100" stroke="blue" stroke-width="2" /> 
```

2. d 以M绝对定位开头，则后面的m相对于前一个M/m    
``` html
示例：
<svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="200" height="200">
    <path d="M20 20 m 0 0 h 320 m -320 0 m 0 20 h 320 m -320  0 m 0 20 h 320" stroke="blue" stroke-width="2" />
</svg>
```

解析：
* M 20 20是第一个起始点
* m 0 0相对于M 20 20
* m -320 0 相对于 m 0 0
* 以此类推...

## 三、js动态添加svg的<path>
``` js
let nameSpace = 'http://www.w3.org/2000/svg'; //定义命名空间
var barChart = document.createElementNS(nameSpace,'svg'); //在html里定义一个svg
barChart.setAttribute('width', barChartWidth); //添加长宽
barChart.setAttribute('height', barChartHeight);
let svgWrap = document.querySelector("#svg-wrapper"); //在id名为svg-wrapper的div里添加此svg
svgWrap.appendChild(barChart);
//水平分区线
let hSectionLine = document.createElementNS(nameSpace, "path"); //在svg里添加一个path
let hSectionLinePath = ""; 
for (let i = 0; i < vScaleNum; i++) {
    hSectionLinePath = hSectionLinePath + " m 0" + (-vScaleSpacing * radtio) + " h " + hLength + " m " + (-hLength) + " 0";
} // 得到的是一个累加的多重定向path
let hSectionLinesPath = zeroPoint + hSectionLinePath;
console.log(hSectionLinesPath); // 类似M20 20 m 0 0 h 320 m -320 0 m 0 20 h 320 m -320  0 m 0 20 h 320
hSectionLine.setAttribute("d", hSectionLinesPath);  //给path新增一个属性，即路径d
hSectionLine.setAttribute("stroke", hSectionLineColor); //给path一个颜色
barChart.appendChild(hSectionLine); //添加至svg节点里
```
