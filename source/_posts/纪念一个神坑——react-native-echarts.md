---
title: 纪念一个神坑rn echarts
date: 2019-03-10 13:47:12
tags: react-native
categories: 调试测试  
---

## 一、问题

在rn项目里引用的时候，本该显示图表的界面显示出了一堆html...

## 二、原因

官方没给配置好

## 三、解决

1. /node_modules/native-echarts/src/components/Echarts/ 目录下的tpl.html 拷贝一份 
2. /android／app/src/main 创建 assets文件夹
3. 把第一步拷贝的文件放到第二步创建的assets文件夹下
4. 进入Echarts文件(/node_modules/native-echarts/src/components/Echarts/index) 把WebView的source改为
``` js
source={{uri: 'file:///android_asset/tpl.html'}}
```

参考文献：[https://segmentfault.com/a/1190000015758777](https://segmentfault.com/a/1190000015758777)