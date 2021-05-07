---
title: canvas闪屏问题的处理
date: 2019-04-14 17:31:12
tags: canvas
categories: 前端  
---

## 一、问题描述

在画canvas时，遇到屏幕瞬间空白的情况（大约1~2帧），造成用户体验不好。

## 二、原因

canvas的绘图过程是：先擦出整个画布；然后浏览器到达重绘时间点后，在空白的canvas上作画；xx毫秒后，这一帧动画上的所有元件完成绘画。

那么，当采用setTimeout或setInterval等与浏览器重绘频率不同步的计时器对画布进行绘图时，很可能上一帧的元件内容还没被完全画出来时，不精准的计时器已经驱动着擦除画布开始下一帧绘画；以及这一帧绘画已经结束，但不精准的计时器还没到指定刷新时间，所以此时会出现明显空白，也即“闪屏”。

## 三、解决

双缓冲理论：闪烁是图形编程的一个常见问题。需要多重复杂绘制操作的图形操作会导致呈现的图像闪烁或具有其他不可接受的外观。双缓冲的使用解决这些问题。双缓冲使用内存缓冲区来解决由多重绘制操作造成的闪烁问题。当启用双缓冲时，所有绘制操作首先呈现到内存缓冲区，而不是屏幕上的绘图图面。所有绘制操作完成后，内存缓冲区直接复制到与其关联的绘图图面。因为在屏幕上只执行一个图形操作，所以消除了由复杂绘制操作造成的图像闪烁。

即，先创建一个cacheCanvas执行计算绘制的操作，当下一帧刷新时，直接将cacheCanvas的内容drawImage到真正的canvas上，这么做既能有效规避闪屏，又能减缓卡顿情况。

示例代码：

``` html
var testBox = function(){
  var canvas = document.getElementById("cas"),
  ctx = canvas.getContext('2d'),
  borderWidth = 2,
  Balls = [];
  var ball = function(x , y , vx , vy , useCache){
    this.x = x;
    this.y = y;
    this.vx = vx;
    this.vy = vy;
    this.r = getZ(getRandom(20,40));
    this.color = [];
    this.cacheCanvas = document.createElement("canvas");
    this.cacheCtx = this.cacheCanvas.getContext("2d");
    this.cacheCanvas.width = 2*this.r;
    this.cacheCanvas.height = 2*this.r;
    var num = getZ(this.r/borderWidth);
    for(var j=0;j<num;j++){
      this.color.push(
        "rgba("+getZ(getRandom(0,255))+",
        "+getZ(getRandom(0,255))+",
        "+getZ(getRandom(0,255))+",1)");
    }
    this.useCache = useCache;
    if(useCache){
      this.cache();
    }
  }

  function getZ(num){
    var rounded;
    rounded = (0.5 + num) | 0;
    // A double bitwise not.
    rounded = ~~ (0.5 + num);
    // Finally, a left bitwise shift.
    rounded = (0.5 + num) << 0;
    return rounded;
  }

  ball.prototype = {
    paint:function(ctx){
      if(!this.useCache){
        ctx.save();
        var j=0;
        ctx.lineWidth = borderWidth;
        for(var i=1;i<this.r;i+=borderWidth){
          ctx.beginPath();
          ctx.strokeStyle = this.color[j];
          ctx.arc(this.x , this.y , i , 0 , 2*Math.PI);
          ctx.stroke();
          j++;
        }
        ctx.restore();
      } else{
        ctx.drawImage(this.cacheCanvas , this.x-this.r , this.y-this.r);
      }
    },

    cache:function(){
      this.cacheCtx.save();
      var j=0;
      this.cacheCtx.lineWidth = borderWidth;
      for(var i=1;i<this.r;i+=borderWidth){
        this.cacheCtx.beginPath();
        this.cacheCtx.strokeStyle = this.color[j];
        this.cacheCtx.arc(this.r , this.r , i , 0 , 2*Math.PI);
        this.cacheCtx.stroke();
        j++;
      }
      this.cacheCtx.restore();
    },

    move:function(){
      this.x += this.vx;
      this.y += this.vy;
      if(this.x>(canvas.width-this.r)||this.x<this.r){
        this.x=this.x<this.r?this.r:(canvas.width-this.r);
        this.vx = -this.vx;
      }
      if(this.y>(canvas.height-this.r)||this.y<this.r){
        this.y=this.y<this.r?this.r:(canvas.height-this.r);
        this.vy = -this.vy;
      }

      this.paint(ctx);
    }
  }

  var Game = {
    init:function(){
      for(var i=0;i<1000;i++){
        var b = new ball(
          getRandom(0,canvas.width) , 
          getRandom(0,canvas.height) , 
          getRandom(-10 , 10) ,  
          getRandom(-10 , 10) , 
          true)
        Balls.push(b);
      }
    },

    update:function(){
      ctx.clearRect(0,0,canvas.width,canvas.height);
      for(var i=0;i<Balls.length;i++){
        Balls[i].move();
      }
    },

    loop:function(){
      var _this = this;
      this.update();
      RAF(function(){
        _this.loop();
      })
    },

    start:function(){
      this.init();
      this.loop();
    }
  }

  window.RAF = (function(){
    return window.requestAnimationFrame
    || window.webkitRequestAnimationFrame
    || window.mozRequestAnimationFrame
    || window.oRequestAnimationFrame
    || window.msRequestAnimationFrame
    || function (callback) {window.setTimeout(callback, 1000 / 60); };
  })();

  return Game;
}();

function getRandom(a , b){
  return Math.random()*(b-a)+a;
}

window.onload = function(){
  testBox.start();
}
```
