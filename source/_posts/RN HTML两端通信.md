---
title: RN HTML两端通信
date: 2019-05-21 10:29:10
tags: 
  - JavaScript 
  - React Native
  - webview
categories: 移动端
---
## 一、RN传数据

1. RN向HTML传数据：
> this.webview.postMessage('"Hello" 我是RN发送过来的数据');

2. HTML接收RN传来的数据：
``` js
document.addEventListener('message', function(e) {
    document.getElementsByTagName('p')[1].innerHTML = e.data;
});
```

## 二、HTML传数据
1. HTML向RN传数据：
> window.postMessage('这是html发送到RN的消息');

2. RN接收HTML传来的数据：
> handleMessage = event => {
   const action = JSON.parse(event.nativeEvent.data)   
   this.setState({ height: action.height, width: action.width })                
  } 

参考：[https://www.jianshu.com/p/9e6f1569227f](https://www.jianshu.com/p/9e6f1569227f)