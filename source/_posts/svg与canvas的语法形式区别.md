---
title: svg与canvas的语法形式区别
date: 2019-07-09 23:05:06
tags:  
  - JavaScript
  - svg
  - canvas
categories: 前端
---
因为网上关于svg和canvas的区别大多是性能上的，关于语法形式的区别资料很少，还有很多错误...导致转换时查资料花了不少时间，因此整理一下，方便对比学习，少走弯路。

## 一、创建概念
canvas是创建出一个画布（ctx），然后一直往画布里添加元素，所以只需要往节点树里添加一次即可；
svg是每创建一个元素，就生成一个新的dom节点，因此每次创建后都要添加节点至节点树。

## 二、语法区别
1. 初始化背景布
canvas
``` js
let canvas = document.createElement('canvas');
let ctx = canvas.getContext("2d");
canvas.style.width = '100%';
canvas.style.height = '100%';
canvas.id = 'canvasOverview';
// 添加至主节点
this.divMain.appendChild(canvas); 
```

svg
``` js
let nameSpace = 'http://www.w3.org/2000/svg';
let svgChart = document.createElementNS(nameSpace,'svg');
svgChart.setAttribute('width', '100%');
svgChart.setAttribute('height', '100%');
svgChart.id = 'svgChart';
// 添加至主节点
this.divMain.appendChild(svgChart);
```

2. 绘制元素
（1）image
canvas
``` js
ctx.drawImage(_this.image, _this.x, _this.y, _this.width, _this.height);
```

Svg
``` js
Let svgImage = document.createElementNS(nameSpace, "image");
svgImage.setAttribute("href", _this.image.src);
svgImage.setAttribute("x", -_this.x);     
svgImage.setAttribute("y", -_this.y);
svgImage.setAttribute("width", _this.width);
svgImage.setAttribute("height", _this.height);
// 添加至节点树
svgChart.appendChild(svgImage);
```

（2）text
Canvas
``` js
ctx.fillStyle = “white”
ctx.fillText(_this.text, _this.x + _this.width / 2, _this.y + _this.height / 2);
ctx.textAlign = "left" // right, center
```

svg
``` js
Let svgText = document.createElementNS(nameSpace, "text");
svgText.setAttribute("fill", “white”);
svgText.textContent = _this.text;
svgText.setAttribute("x", _this.x + _this.width / 2 );   
svgText.setAttribute("y", _this.y + _this.height / 2 );
svgText.setAttribute("text-anchor", "start"); // end, middle
// 添加至节点树
svgChart.appendChild(svgText);
```

（3）image transform
Canvas
``` js
ctx.translate(_this.x, _this.y);
ctx.translate(_this.width / 2, _this.height / 2);
ctx.rotate(_this.rotate * Math.PI / 180);
```

Svg
``` svg
let transformImage = document.createElementNS(nameSpace, "image"); 
transformImage.setAttribute('transform', `matrix(
    ${Math.cos(_this.rotate * Math.PI / 180)},
    ${Math.sin(_this.rotate * Math.PI / 180)},
    ${-Math.sin(_this.rotate * Math.PI / 180)},
    ${Math.cos(_this.rotate * Math.PI / 180)},
    ${Math.floor(_this.x + _this.width / 2)},
    ${Math.floor(_this.y + _this.height / 2)})`
);
// 添加至节点树
svgChart.appendChild(transformImage);
```

（4）line
Canvas
``` js
ctx.beginPath();
ctx.moveTo(this.startX, this.startY);
ctx.lineTo(this.endX, this.endY);
ctx.closePath();
ctx.stroke();
ctx.restore();
```

Svg
``` js
let line = document.createElementNS(nameSpace, "path");
let linePath = "M " + this.startX + " " + this.startY + " " + "L " + this.endX + " " + this.endY + " " + "Z";
line.setAttribute("d", linePath);
line.setAttribute("stroke-width", this.lineWidth);
// 添加至节点树
svgChart.appendChild(line);
```

（5）circle
Canvas
``` js
ctx.fillStyle = '#017db6';
ctx.save();
ctx.fillStyle = "#6fc88f";
ctx.beginPath();
//ctx.arc(this.pointX, this.pointY, this.radius, 0, Math.PI * 2);
ctx.arc(this.pointX, this.pointY, this.lineWidth / 2 - 1, 0, Math.PI * 2);
ctx.closePath();
ctx.fill();
ctx.restore();
```

Svg
``` js
let circle = document.createElementNS(nameSpace, "circle");
circle.setAttribute("fill", "#6fc88f");
circle.setAttribute("cx", this.pointX);
circle.setAttribute("cy", this.pointY);
circle.setAttribute("r", this.lineWidth / 2 - 1);
// 添加至节点树
svgChart.appendChild(circle);
```

3. 清除画布
canvas
``` js
ctx.clearRect(0, 0, _this.canvas.width, _this.canvas.height);
```

svg
``` js
let svgWrap = document.getElementById("divMain");
let beforeSVG = document.getElementById("svgChart");
if (beforeSVG) {
  svgWrap.removeChild(beforeSVG);
}
```