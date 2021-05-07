---
title: RN canvas画布大小之谜
date: 2019-04-02 09:49:10
tags: 
  - JavaScript 
  - React Native
  - canvas
categories: 移动端
---

## 一、需求

在一个高640、宽360的canvas内画一些坐标点。

## 二、问题

坐标点只显示了一部分，剩下的点没显示（其坐标属于（640,360）区域）。

## 三、原因

canvas默认的画布大小是高150，宽300，这个范围以外的点自然显示不了，因此判断是画布大小的设置有问题。

设置画布大小的时候，分别用两种方式设置width和height：

1.style
  Canvas 把自己这块地作为默认参数canvas传给handleCanvas
``` html
<Canvas 
  style={{ 
    width: 360,
    height: 640,
    backgroundColor: '#F5FCFF'
  }}
  ref={this.handleCanvas}
/>     
```

2.props
  Canvas 把自己这块地作为默认参数canvas传给handleCanvas
``` html
<Canvas 
  width={360}
  height={640}
  style={{ 
    backgroundColor: '#F5FCFF'
  }}
  ref={this.handleCanvas}
/>     
```
结果，并没有改变什么...

深层原因：上述两种方案都仅仅改变的是canvas的props值
![avatar](https://img2018.cnblogs.com/blog/1549437/201904/1549437-20190402094159554-1867370404.png)

而决定画布的大小是attribute值，而非property值，而前两种方案修改的都是property值。attribute值如下图：

![avatar](https://img2018.cnblogs.com/blog/1549437/201904/1549437-20190402094649835-2049879035.png)

仍旧是默认的300和150。

## 四、解决

用attribute的方法修改attribute

handleCanvas = (canvas) => {
  canvas.height = 640;
  canvas.width = 360;
  this.setState({ canvas });
}  
结果：
![avatar](https://img2018.cnblogs.com/blog/1549437/201904/1549437-20190402094820623-1207144052.png)

attribute被修改，所有的点正常显示。

附一篇PC端的canvas解决方案：[https://blog.csdn.net/csm0912/article/details/52963240](https://blog.csdn.net/csm0912/article/details/52963240)

