---
title: vue环境中跨域问题的处理
date: 2020-04-20 23:49:10
tags: 
  - JavaScript
  - vue 
  - 跨域
categories: 前端
---

## 一、vue.config.js
``` js
// vue.config.js
module.exports = {
  publicPath: 'project',
  outputDir: 'docs',
  lintOnSave: false,
  devServer: {
    host: 0.0.0.0,
    port: 8080,
    proxy: {
      '/api': {
        target: 'http://localhost:3000',
        changeOrigin: true
      }
    }
  }
};
```
假设后端请求地址是localhost:3000，配置devServer的proxy即可，但这个方式只能在开发环境中使用。

## 二、cors
后端操作，服务器开启跨域资源共享
``` js
const cors = require('koa2-cors'); // 解决跨域的中间件 koa2-cors
const app = new Koa();
app.use(cors({
  origin: function(ctx) {
    return ctx.header.origin
  }, // 允许发来请求的域名
  allowMethods: [ 'GET', 'POST', 'PUT', 'DELETE', 'OPTIONS' ], // 设置所允许的 HTTP请求方法
  credentials: true, // 标示该响应是合法的
  allowHeaders: ['Content-Type', 'Authorization', 'Accept', 'Content-Length'],
}));
```
注意前端也需要设置 credentials：
axios.defaults.withCredentials = true; // 表示跨域请求时是否需要使用凭证

## 三、nginx代理
配置nginx，使其代理后端端口返回数据
``` js
server {
  listen  80;
  server_name  originServer;

  location / {
    root /project/;
    index index.html;
    try_files $uri $uri/ /index.html;
    proxy_pass  http://127.0.0.1:3000/;
  }
```

## 四、http-proxy-middleware代理
``` js
var proxy = require('http-proxy-middleware')
var app = express()

app.use('/api', proxy({
  target: 'http://xxxxx', // 目标代理地址
  changeOrigin: true,
  pathRewrite: {
    '^/api': ''
  }  
}))
```