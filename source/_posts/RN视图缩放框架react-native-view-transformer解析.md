---
title: RN视图缩放框架react-native-view-transformer解析
date: 2019-05-11 22:16:10
tags: 
  - JavaScript 
  - React Native
categories: 移动端
---

## 一、组成
结构：
> ->library
　　->transform
　　　　->Rect.js
　　　　->TransformUtils.js
　　　　->ViewTransformer.js
　　->index.js
　　->.npmignore
　　->package.json
　　->README.md

依赖：
``` json
"dependencies": {
  "react-native-gesture-responder": "0.1.1",
  "react-native-scroller": "0.0.6"
}
```

react-native-view-transformer（后文简称‘vt’），是基于RN手势系统和滑轮做的一个视图封装组件。含有放大缩小、双击和拖动控制功能。

## 二、功能解析
vt主要是由transform下的三个文件创造而成。其中，Rect负责构造基本矩形；TransformUtils控制矩形转换的样式；ViewTransformer控制矩形转换的属性。

控制缩放比例：ViewTransformer.js->animateBounce()

``` js
animateBounce() {
  let curScale = this.state.scale;
  //最小最大缩放倍数  
  let minScale = 0.2;
  let maxScale = this.props.maxScale;
  let scaleBy = 1;
  if (curScale > maxScale) {
    scaleBy = maxScale / curScale;
  } else if (curScale < minScale) {
    scaleBy = minScale / curScale;
  }

  let rect = transformedRect(this.transformedContentRect(), new Transform(
    scaleBy,
    0,
    0,
    {
      x: 0,
      y: 0
    }
  ));

  rect = alignedRect(rect, this.viewPortRect(), scaleBy);
  this.animate(rect);
}
```

控制界面移动速度：ViewTransformer.js->applyResistance() 

``` js
applyResistance(dx, dy) {
  let availablePanDistance = availableTranslateSpace(this.transformedContentRect(), this.viewPortRect());

  if ((dx > 0 && availablePanDistance.left < 0)
    ||
    (dx < 0 && availablePanDistance.right < 0)) {
　　//手势横向移动距离
    dx /= 1;
  }
  if ((dy > 0 && availablePanDistance.top < 0)
    ||
    (dy < 0 && availablePanDistance.bottom < 0)) {
    //手势纵向移动距离   
    dy /= 1;
  }
  return {
    dx, dy
  }
}
```

控制双击放大倍数：ViewTransformer.js->performDoubleTapUp()
``` js
performDoubleTapUp(pivotX, pivotY) {
  console.log('performDoubleTapUp...pivot=' + pivotX + ', ' + pivotY);
  let curScale = this.state.scale;
  //控制每次双击放大的倍数
  let scaleBy = 1;

  let rect = transformedRect(this.transformedContentRect(), new Transform(
    scaleBy, 0, 0,
    {
      x: pivotX,
      y: pivotY
    }
  ));
  
  rect = transformedRect(rect, new Transform(1, this.viewPortRect().centerX() - pivotX, this.viewPortRect().centerY() - pivotY));
  rect = alignedRect(rect, this.viewPortRect());

  this.animate(rect);
}
```

控制界面边界限制：ViewTransformer.js->animateBounce() -> TransformUtils.js-> alignedRect()

``` js
export function alignedRect(rect:Rect, viewPortRect:Rect, scaleBy) {
  let dx = 0, dy = 0;
    //控制左右边界
    if (rect.left > viewPortRect.left) {
      dx = viewPortRect.left - rect.left;
    } else if (Math.abs(rect.right) > (2000*scaleBy - 720)) {
      dx = 2000*scaleBy;
    }
    
    //控制上下边界
    if (rect.top > viewPortRect.top) {
      dy = viewPortRect.top - rect.top;
    } else if (Math.abs(rect.bottom) > 1000) {
      dy = 340;
    }

  return rect.copy().offset(dx, dy);
}
```

控制界面展示中心：TransformUtils.js-> fitCenterRect()
``` js
export function fitCenterRect(contentAspectRatio, containerRect:Rect) {
  let w = containerRect.width();
  let h = containerRect.height();
  let viewAspectRatio = w / h;

  if (contentAspectRatio > viewAspectRatio) {
    h = w / contentAspectRatio;
  } else {
    w = h * contentAspectRatio;
  }

  //控制新界面的布局中心点  
  return new Rect(
    containerRect.centerX() - w / 2,
    containerRect.centerY() - h / 2,
    containerRect.centerX() + w / 2,
    containerRect.centerY() + h / 2
  );
}
```
