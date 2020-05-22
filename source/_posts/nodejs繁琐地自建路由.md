---
title: nodejs繁琐地自建路由
date: 2019-01-05 11:50:08
tags: NodeJs
categories: 后端
---

## 一、繁琐的自建路由
app.js
``` js 
var server = require('./server');
server.startServer();
```

server.js
``` js
var http = require('http');
var fs = require('fs');

function startServer () {
  var onRequest = function(request, response) {
    if (request.url === '/' || request.url === '/home') {
      response.writeHead(200, { 'Content-Type': 'text/html'});
      fs.createReadStream(__dirname + '/index.html', 'utf8').pipe(response);
    } else if (request.url === '/review') {
      response.writeHead(200, { 'Content-Type': 'text/html'});
      fs.createReadStream(__dirname + '/review.html', 'utf8').pipe(response);
    } else if (request.url === '/api/v1/records') {
      response.writeHead(200, { 'Content-Type': 'application/json' });
      var jsonObj = {
        name: 'sfafas',
        job: 'coder'
      }
      response.end(JSON.stringify(jsonObj));
    } else {
      response.writeHead(200, {'Content-Type': 'text/html'});
      fs.createReadStream(__dirname + '/404.html', 'utf8').pipe(response);
    }
  }
  
  var server = http.createServer(onRequest);
  server.listen(3000);    
} 

console.log('finished');
module.exports.startServer = startServer;
```

## 二、重构路由
app.js
``` js
var server = require('./server');
var router = require('./router');

var handler = require('./handler');

var handle = {};
handle['/'] = handler.home;
handle['/home'] = handler.home;
handle['/review'] = handler.review;
handle['/api/v1/records'] = handler.api_records;

server.startServer(router.route,handle);
```

server.js
``` js
var http = require('http');
var fs = require('fs');

function startServer (route,handle) {
  var onRequest = function(request, response) {
    route(handle, request.url,response);
  }
  
  var server = http.createServer(onRequest);
  server.listen(3000);    
} 

console.log('finished');
module.exports.startServer = startServer;
```

handler.js
``` js
var fs = require('fs');

function home(response) {
  response.writeHead(200, { 'Content-Type': 'text/html'});
  fs.createReadStream(__dirname + '/index.html', 'utf8').pipe(response);
}

function review(response) {
  response.writeHead(200, { 'Content-Type': 'text/html'});
  fs.createReadStream(__dirname + '/review.html', 'utf8').pipe(response);
}

function api_records(response) {
  response.writeHead(200, { 'Content-Type': 'application/json' });
  var jsonObj = {
    name: 'sfafas',
    job: 'coder'
  }
  response.end(JSON.stringify(jsonObj));
}

module.exports = {
  home: home,
  review: review,
  api_records: api_records
}
```

router.js
``` js
var fs = require('fs');

function route(handle, pathname,response) {
  console.log('Routing a request for' + pathname);
  if (typeof handle[pathname] === 'function') {
    handle[pathname](response);
  } else {
    response.writeHead(200, {'Content-Type': 'text/html'});
    fs.createReadStream(__dirname + '/404.html', 'utf8').pipe(response);
  }
}

module.exports.route = route;
```

## 三、页面整体结构
![avatar](https://img2018.cnblogs.com/blog/1549437/201901/1549437-20190105114901882-1723734341.png)


