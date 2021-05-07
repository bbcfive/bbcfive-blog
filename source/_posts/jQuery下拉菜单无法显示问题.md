---
title: jQuery下拉菜单无法显示问题
date: 2018-12-17 17:04:00
tags: 
  - JavaScript
  - jQuery
categories: 调试测试
---
## 一、问题描述

做下拉菜单时，不管怎么触发事件，下拉菜单都不显示。console一下，发现其display一直是none。

``` css
.second>li{
    width: 300px;
    height: 30px;
    list-style: none;
    background-color: grey;
    color: #fff;
    border-bottom: .5px #fff solid;
    /*页面刷新时不显示，触发事件后显示*/
    display: none;        
}
```

## 二、问题发现

原因是因为，“display: none;”放错地方了，跟li放一起。但事件触发的是父元素ul，所以无法更改其displaynone属性。

## 三、解决
``` css
/*display: none;单独放出来，不要跟li放一起*/
.second{
    display: none;                
}
```
新开一个块控制父级元素，用对应的展开/收起动画控制即可。