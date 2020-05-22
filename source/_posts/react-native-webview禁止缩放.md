---
title: react-native-webview禁止缩放
date: 2019-06-07 22:13:39
tags: 
  - JavaScript 
  - React Native
  - webview
categories: 移动端
---
## 一、需求

RN-webview禁止缩放，即固定屏幕大小，但同时要能够监听到其他手势识别

## 二、实现

仅仅设置webview的大小自适应是不够的，因为webview所引入的h5页面有可能是能够缩放的。

因此先向h5页面注入代码：

``` js
const INJECTEDJAVASCRIPT = `
  const meta = document.createElement('meta'); 
  meta.setAttribute('content', 'initial-scale=0.5, maximum-scale=0.5, user-scalable=0'); 
  meta.setAttribute('name', 'viewport'); 
  document.getElementsByTagName('head')[0].appendChild(meta); 
`
```

而后设置webview：

``` html
<WebView
  ref={ref => (this.webview = ref)}
  javaScriptEnabled={true}
  scalesPageToFit={false}
  injectedJavaScript={ INJECTEDJAVASCRIPT }
  source={{ uri: this.state.source }}
/>
```

即可固定页面。