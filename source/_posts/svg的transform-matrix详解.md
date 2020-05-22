---
title: svg的transform-matrix详解
date: 2019-07-09 10:33:08
tags: 
  - JavaScript 
  - svg
categories: 前端
---

## 一、svg transform的种类
translate（平移）
skew（倾斜）
rotate（旋转）
scale（缩放）
matrix（矩阵操作，可涵盖前四者，威力十分强大）

## 二、用matrix表达前三者
#### matrix接口样式：matrix(a,b,c,d,e,f)

#### 对应矩阵：

a  c  e
b  d  f
0  0  1

1. Translate(tx, ty) 

#### 矩阵：

1  0  tx
0  1  ty
0  0  1

#### 写法：matrix(1,0,0,1,tx,ty)

2. Scale(sx, sy)

#### 矩阵：

sx  0   0
0   sy  0
0   0   1

#### 写法：matrix(sx,0,0,sy,0,0)

3. Rotate(a) 

#### 矩阵：

cos(a)  -sin(a)  0
sin(a)  cos(a)   0
0       0        1

#### 写法：matrix(cos(a), sin(a), -sin(a), cos(a), 0, 0)

3+. Rotate(a, cx, cy) 

#### 矩阵

cos(a)  -sin(a)  -cx × cos(a) + cy × sin(a) + cx
sin(a)   cos(a)  -cx × sin(a) - cy × cos(a) + cy
0        0       1

#### 写法：matrix(cos(a), sin(a), -sin(a), cos(a),  -cx × cos(a) + cy × sin(a) + cx + tx, -cx × sin(a) - cy × cos(a) + cy + ty)

4. skew(skewX, skewY)

4.1 skewX

#### 矩阵：

1  tan(a)    0
0     1      0
0     0      1

#### 写法：matrix(1, 0, tan(a), 1, 0, 0)

4.2 skewY

#### 矩阵：

1         0     0
tan(a)    1     0
0         0     1

#### 写法：matrix(1, tan(a), 0, 1, 0, 0)

5. 如果你的使用包含旋转，缩放，平移等多种

例如：
``` js
<g transform="translate(20, 50) scale(1, 1) rotate(-30 10 25)">
```

#### 对应矩阵：

cos(a)  -sin(a)  -cx × cos(a) + cy × sin(a) + cx + tx
sin(a)   cos(a)  -cx × sin(a) - cy × cos(a) + cy + ty
0        0       1

#### 写法：matrix(cos(a), sin(a), -sin(a), cos(a), -cx × cos(a) + cy × sin(a) + cx + tx, -cx × sin(a) - cy × cos(a) + cy + ty)

以上式为例: matrix(0.866, -0.5 0.5 0.866 8.84 58.35).

此处scale为1，因此不考虑。如果scale不为一（scale (sx, sy)），则：

matrix (sx × cos(a), sy × sin(a), -sx × sin(a), sy × cos(a), (-cx × cos(a) + cy × sin(a) + cx) × sx + tx, (-cx × sin(a) - cy × cos(a) + cy) × sy + ty)

最后，在js中的格式为：

``` js
svgItem.setAttribute('transform', `matrix(
  ${sx * Math.cos(a)},
  ${sy * Math.sin(a)},
  ${-sx * Math.sin(-a)},
  ${sy * Math.cos(a)},
  ${(-cx * Math.cos(a) + cy * Math.sin(a) + cx) * sx + Math.floor(tx)},
  ${(-cx * Math.sin(a) - cy * Math.cos(a) + cy) * sy + Math.floor(ty)})`
);
```