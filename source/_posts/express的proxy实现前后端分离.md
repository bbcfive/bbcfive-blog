---
title: express的proxy实现前后端分离
date: 2019-01-13 17:28:08
tags: 
  - express
  - NodeJs
  - 跨域
categories: 后端
---

``` js
var express = require('express')
var proxy = require('http-proxy-middleware')
var app = express()

app.use('/api', proxy({
  target: 'http://xxxxx', // 目标代理地址
  changeOrigin: true,
  pathRewrite: {
    '^/api': ''
  }  
}))

app.use(express.static('dist'))

app.get('*', function(req, res) {
  res.sendfile('./dist/index.html')
}) 

app.listen(8080, function(){
  do sth. 
})
```



