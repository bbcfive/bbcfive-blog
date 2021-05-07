---
title: React Native获取手机的各种高度
date: 2019-07-02 20:49:10
tags: 
  - JavaScript 
  - React Native
categories: 移动端
---
![avatar](https://img2018.cnblogs.com/blog/1549437/201907/1549437-20190702204230511-278531161.jpg)

## 一、窗口高度
即图中黄色+蓝色部分
``` js
import { Dimensions } from "react-native";
const deviceHeight = Dimensions.get('window').height;  
```

## 二、屏幕高度
即图中黄色+蓝色+红色部分
``` js
import { Dimensions } from "react-native";
const deviceHeight = Dimensions.get('screen').height;  
```

## 三、内容高度
即图中蓝色部分
``` js
import { StyleSheet, View, Text, ScrollView, Dimensions, PanResponder, StatusBar } from "react-native";
const deviceHeight = Dimensions.get('window').height;  
const STATUS_BAR_HEIGHT = StatusBar.currentHeight; //即图中黄色部分
const height = deviceHeight  - STATUS_BAR_HEIGHT;
```
