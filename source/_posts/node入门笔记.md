---
title: node入门笔记
date: 2019-01-04 18:37:08
tags: NodeJs
categories: 后端
---

## 一、Node简介

node是一个基于v8引擎的JavaScript运行环境。

## 二、全局对象

setTimeout
setInterval
clearTimeout
clearInterval
console
//当前目录下的文件
console.log(__dirname);
//当前文件名
console.log(__filename);

## 三、模块
app.js
``` js
//模块
var obj = require('./count.js');
console.log(obj.arr);
console.log(obj.adder(3,2));
console.log('there is ' + obj.arr.length + ' fruits in the packet');
```

count.js
``` js
var arr = ['apple', 'banana', 'orange'];

var adder = function(a,b) {
  return `this result is ${a+b}`;
}

/* module.exports.arr = arr;
module.exports.adder = adder; */

//等价于,这里exports出了一个对象
module.exports = {
  arr: arr,
  adder: adder
}
```

## 四、事件

``` js
//事件核心库
var events = require('events');
//新增事件
var myEmitter = new events.EventEmitter();
//给事件绑定函数
myEmitter.on('someEvent', function(message) {
    console.log(message);
})
//触发事件
myEmitter.emit('someEvent', 'the event was emmited');
```

``` js
//导入一个工具库
var util = require('util');

var Person = function(name) {
  this.name = name
}

//继承所有Person的事件
util.inherits(Person, events.EventEmitter);

var xiaoming = new Person('xiaoming');
var lili = new Person('lili');
var lucy = new Person('lucy');

var Person = [xiaoming, lili, lucy];
//绑定事件
Person.forEach(person => {
  person.on('speak', message => {
    console.log(person.name + ' said ' + message);
  })
});
//触发事件
xiaoming.emit('speak','hi');
lili.emit('speak','hello');
lucy.emit('speak','nihao');
``` 

## 五、读写文件（同步、异步）
``` js
var fs = require('fs');
//同步读取文件
var readMe = fs.readFileSync('./test1/readMe.txt','utf8');
//同步写文件
fs.writeFileSync('writeMe.txt', readMe);
//异步读取文件
fs.readFile('./test1/readMe.txt','utf8', function(err,data){
  console.log(data);
});
//异步写文件
fs.writeFile('writeMe2.txt', readMe,function(err,data){
  console.log('data is copyed');
});
```

## 六、创建和删除文件

``` js
//创建文件
fs.mkdir('stuff',function(){
  fs.readFile('readMe.txt', 'utf8',function(err,data){
    fs.writeFile('./stuff/writeMe.txt',data,function(){
      console.log('copy successfully');
    })
  })
});

//删除文件
fs.unlink('readMe.txt',function(){
  console.log('this file is deleted');
});
```

## 七、流和管道

ls => 查看当前目录下的文件

ls | grep xx => 查看当前目录下是否含有xx文件

``` js
var fs = require('fs');
var myReadStream = fs.createReadStream(__dirname + '/test1/readMe.txt', 'utf8');
var myWriteStream = fs.createWriteStream(__dirname + '/test1/writeMe.txt','utf8');
var data = '';
myReadStream.on('data', function(chunk) {
  //data += chunk;
  myWriteStream.write(chunk);
})

myReadStream.on('end', function() {
  //console.log(data);
})
```

``` js
var fs = require('fs');
var myReadStream = fs.createReadStream(__dirname + '/test1/readMe.txt', 'utf8');
var myWriteStream = fs.createWriteStream(__dirname + '/test1/writeMe.txt','utf8');

var writeData = 'hello world';
myWriteStream.write(writeData);
myWriteStream.end();
myWriteStream.on('finish', function() {
  console.log('finished');
})

//用管道实现复制文件
myReadStream.pipe(myWriteStream);
```

## 八、web服务器

创建一个本地的服务器：

``` js
var http = require('http');
var server = http.createServer(function(request, response) {
  response.writeHead(200, { 'Content-Type': 'text/plain'});
  //response.write('Hello from out application');
  response.end('Hello from out application');
})
server.listen(3000, '127.0.0.1');
```

传入纯文本：

``` js
var http = require('http');
var onRequest = function(request, response) {
  response.writeHead(200, { 'Content-Type': 'text/plain'});
  //response.write('Hello from out application');
  response.end('Hello world');
}
var server = http.createServer(onRequest);
server.listen(3000, '127.0.0.1');
console.log('Server started at ls port:3000');
``` 

传入json对象：

``` js
var http = require('http');
var onRequest = function(request, response) {
  response.writeHead(200, { 'Content-Type': 'text/json'});
  //response.write('Hello from out application');
  var myObj = {
    name: 'sasbfhad',
    job: 'coder',
    age: '21'
  };
  response.end(JSON.stringify(myObj));
}
var server = http.createServer(onRequest);
server.listen(3000, '127.0.0.1');
console.log('Server started at ls port:3000');
``` 

传html：

``` js
var http = require('http');
var fs = require('fs');
var onRequest = function(request, response) {
  response.writeHead(200, { 'Content-Type': 'text/html'});
  //response.write('Hello from out application');
  var myReadStream = fs.createReadStream(__dirname + '/shiyan.html', 'utf8');
  myReadStream.pipe(response);
}

var server = http.createServer(onRequest);
server.listen(3000, '127.0.0.1');
console.log('Server started at ls port:3000');
```

封装：

app.js
``` js
var server = require('./server');
server.startServer();
```

server.js

``` js
var http = require('http');
var fs = require('fs');
function startServer() {
  var onRequest = function(request, response) {
    response.writeHead(200, { 'Content-Type': 'text/plain'});
    //response.write('Hello from out application');
    var myReadStream = fs.createReadStream(__dirname + '/shiyan.html', 'utf8');
    myReadStream.pipe(response);
  }    

  var server = http.createServer(onRequest);
  server.listen(3000, '127.0.0.1');    
  console.log('Server started at ls port:3000');
}

module.exports.startServer = startServer;
```