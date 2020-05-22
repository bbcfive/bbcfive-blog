---
title: css知识体系搭建
date: 2018-12-08 09:58:12
tags: 
  - css
  - 基础知识
categories: 前端  
---

![avatar](https://img2018.cnblogs.com/blog/1549437/201812/1549437-20181208095701469-204932820.png)

## 一、选择器

1. 基本选择器：

  * 通用元素选择器
  * 标签选择器
  * 类选择器
  * id选择器

2. 组合选择器：

  * 多元素组合选择器
  * 后代元素选择器
  * 子代元素选择器
  * 毗邻元素选择器

3. 属性选择器：

  * [title] & P[title]
  * [title=mk]
  * [title~="mk"]
  * [title|=mk]
  * [title^=Nick]
  * [title$=Nick]
  * [title*=Nick]

4. 伪类选择器：

  * link、hover、active、visited
  * before、after
 
#### 属性选择器

1.[title] & P[title]

  设置所有具有title属性的标签元素；
  设置所有具有title属性的P标签元素。

``` html
[title]
  {
    color: yellow;
  }
  p[title]
  {
    color: yellow;
  }
<div title>hello</div>
<p title>hello</p>
```

2.[title=mk]

  设置所有title属性等于“mk”的标签元素。 

``` html
[title="mk"]
  {
    color: yellow;
  }
  
<p title="mk">mk</p>
```

3.[title~="mk"]

　　设置所有title属性具有多个空格分隔的值、其中一个值等于“mk”的标签元素。

``` html
[title~="mk"]
  {
    color: yellow;
  }
  
<p title="mk Jenny">Mk</p>
<p title="Jenny mk">Mk</p>
```

4.[title|=mk]　

设置所有title属性具有多个连字号分隔（hyphen-separated）的值、其中一个值以"mk"开头的标签元素。

 例：lang属性："en"、"en-us"、"en-gb"等等

``` html
[title|="mk"]
    {
        color: yellow;
    }
  
<p title="mk-Jenny">mk</p>
```

5.[title^=Nick]

　　设置属性值以指定值开头的每个标签元素。

``` html
[title^="mk"]
  {
    color: yellow;
  }
  
<p title="mkJenny">mk</p>
```

6.[title$=Nick]

　　设置属性值以指定值结尾的每个标签元素。

``` html
[title$="mk"]
  {
    color: yellow;
  }
  
<p title="Jenny mk">mk</p>
```

7.[title*=Nick]

　　设置属性值中包含指定值的每个元素。

``` html
[title*="mk"]
    {
        color: yellow;
    }
  
<p title="SmkJenny">mk</p>
```

#### 伪类选择器：

1. link、hover、active、visited

 * a:link（未访问的链接状态）,用于定义了常规的链接状态。
 * a:hover（鼠标放在链接上的状态）,用于产生视觉效果。
 * a:active（在链接上按下鼠标时的状态）。
 * a:visited（已访问过的链接状态）,可以看出已经访问过的链接。

2. before、after

* P:before 在每个< p >元素的内容之前插入内容;
* P:after 在每个< p >元素的内容之后插入内容。

## 二、属性
1. color 

  * HEX（十六进制色：color: #FFFF00 --> 缩写：#FF0）
  * RGBA（红绿蓝透明度，A是透明度在0~1之间取值。使用方式：color:rgba(255,255,0,0.5)）
  * HSL（CSS3有效,H表示色调，S表示饱和度，L表示亮度，使用方式：color:hsl(360,100%,50%)）
  * HSLA（和HSL相似，A表示Alpha透明度，取值0~1之间。）

transparent
  * 全透明，使用方式：color: transparent
 
2. 文本属性:

  white-space: 设置元素中空白的处理方式
    * normal：默认处理方式。
    * pre：保留空格，当文字超出边界时不换行
    * nowrap：不保留空格，强制在同一行内显示所有文本，直到文本结束或者碰到br标签
    * pre-wrap：保留空格，当文字碰到边界时换行
    * pre-line：不保留空格，保留文字的换行，当文字碰到边界时换行
  
  direction: 规定文本的方向 
    * ltr 默认，文本方向从左到右。
    * rtl 文本方向从右到左。
  
  vertical-align: 文本所在行高的垂直对齐方式
    * baseline 默认
    * sub 垂直对齐文本的下标，和< sub >标签一样的效果
    * super 垂直对齐文本的上标，和< sup >标签一样的效果
    * top 对象的顶端与所在容器的顶端对齐
    * text-top 对象的顶端与所在行文字顶端对齐
    * middle 元素对象基于基线垂直对齐
    * bottom 对象的底端与所在行的文字底部对齐
    * text-bottom 对象的底端与所在行文字的底端对齐
  
  text-indent: 文本缩进

  letter-spacing: 添加字母之间的空白

  word-spacing: 添加每个单词之间的空白

  text-transform: 属性控制文本的大小写
    * capitalize 文本中的每个单词以大写字母开头。
    * uppercase 定义仅有大写字母。
    * lowercase 定义仅有小写字母。
  
  text-overflow: 文本溢出样式
    * clip 修剪文本。
    * ellipsis 显示省略符号...来代表被修剪的文本。
    * string 使用给定的字符串来代表被修剪的文本
  
  text-decoration: 文本的装饰
    * none 默认。
    * underline 下划线。
    * overline 上划线。
    * line-through 中线。
  
  text-shadow：文本阴影
    * 第一个参数是左右位置
    * 第二个参数是上下位置
    * 第三个参数是虚化效果
    * 第四个参数是颜色
    * text-shadow: 5px 5px 5px #888;
  
  word-wrap：自动换行
    * word-wrap: break-word;
  
  去掉li前的点：li{list-style：none}

## 三、背景属性
background-position 设置背景图像的位置坐标
  * background-position: center center; 图片置中，x轴center，y轴center
  * 1px -195px  截取图片某部分，分别代表坐标x，y轴

background-repeat 设置背景图像不重复平铺
  * no-repeat 设置图像不重复，常用
  * round 自动缩放直到适应并填充满整个容器
  * space 以相同的间距平铺且填充满整个容器

background-attachment 背景图像是否固定或者随着页面的其余部分滚动

background 简写
  * background: url("o_ns.png") no-repeat 0 -196px;
  * background: url("o_ns.png") no-repeat center bottom 15px;
  * background: url("o_ns.png") no-repeat left 30px bottom 15px;

## 四、列表属性
list-style-type: 列表项标志的类型
  * none 去除标志
  * decimal-leading-zero;  02.
  * square;  方框
  * circle;  空心圆
  * upper-alph; & disc; 实心圆
  
list-style-image：将图象设置为列表项标志

list-style-position：列表项标志的位置
  * inside
  * outside

## 五、页面布局
#### visibility
  * collapse 当在表格元素中使用时，此值可删除一行或一列，不会影响表格的布局。

#### clip 剪裁图像
rect 剪裁定位元素：
  * auto 默认值，无剪切
  * 上-右-下-左（顺时针）的顺序提供四个偏移值
  * 区域外的部分是透明的
  * 必须指定 position:absolute;
  * 例：clip:rect(0px,60px,200px,0px);

#### overflow  设置当对象的内容超过其指定高度及宽度时如何显示内容
  * visible 默认值，内容不会被修剪，会呈现在元素框之外。
  * hidden 内容会被修剪，并且其余内容是不可见的。
  * scroll 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。
  * auto 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。

#### outline 边框轮廓
  * outline-width 轮廓宽度
  * outline-color 轮廓颜色
  * outline-style 轮廓样式

#### zoom 缩放比例 ：100%/150%/200%

#### transform 转换，变形
  * origin 定义旋转基点（left top center right bottom 坐标值）    transform-origin: 50px 50px; transform-origin: left;。
  * rotate 旋转            transform:rotate(50deg) 旋转角度可以为负数，需要先定义origin。
  * skew  扭曲             transform:skew(50deg,50deg)  分别为相对x轴倾斜,相对y轴倾斜。
  * scale  缩放             transform:scale(2,3) 横向放大2倍，纵向放大3倍；transform:scale(2) 横竖都放大2倍。 
  * translate 移动        transform:translate(50px, 50px) 分别为相对x轴移动,相对y轴移动

#### Transition 平滑过渡
  * transition-property：           变换的属性（none(没有属性改变)、all(所有属性改变)、具体属性）
  * transition-duration：           变换持续时间
  * transition-timing-function： 变换的速率（ease:(逐渐变慢)、linear:(匀速)、ease-in:(加速)、ease-out:(减速)、ease-in-out:(加速然后减速)、cubic-bezier:(自定义时间曲线））
  * transition-delay：               变换延迟时间
  * transition：                        缩写

#### cursor 鼠标的类型形状

url: 自定义光标

Auto: 默认

  Default: 默认
  e-resize
  ne-resize
  nw-resize
  n-resize
  se-resize
  sw-resize
  s-resize
  w-resize
  Crosshair
  Pointer
  Move
  text
  wait
  help