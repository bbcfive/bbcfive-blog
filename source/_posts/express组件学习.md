---
title: express组件学习
date: 2019-01-05 18:05:08
tags: 
  - express
  - NodeJs
categories: 后端
---

## 一、express

可以做：web application、api...

特性：
  * 适合写简单的路由系统
  * 集成很多模板引擎
  * 中间件系统

## 二、请求与响应
``` js
var express = require('express');

var app = express();

//据说第二个参数是一个中间件方法
app.get('/', function(req, res) {
  //学会查api来学习更多知识，学会学习官方api文档
  var resObj = req.method;
  //send可以发送字符串、json（包含自动stringify）对象和数组
  res.send(resObj);
})

app.listen(3000);
console.log('listening to port 3000');
```

## 三、路由参数

server.js

``` js
var express = require('express');

var app = express();

app.get('/profile/:id/user/:name', function(req, res) {
  console.dir(req.params);
  res.send('you requested to a profile page with the name of ' + req.params.name);
})

app.listen(3000);
console.log('listening to port 3000');
```

terminal
![avatar](https://img2018.cnblogs.com/blog/1549437/201901/1549437-20190105190545773-462007949.png)

chrome
![avatar](https://img2018.cnblogs.com/blog/1549437/201901/1549437-20190105190611177-1351317061.png)

## 四、查询字符串

server.js

``` js
app.get('/', function(req, res) {
  console.dir(req.query);
  res.send('home page:' + req.query.find);
})
```

terminal
![avatar](https://img2018.cnblogs.com/blog/1549437/201901/1549437-20190105194552576-1647382851.png)

chrome
![avatar](https://img2018.cnblogs.com/blog/1549437/201901/1549437-20190105194612080-1192770150.png)


## 五、post请求和postman工具

同时处理x-www-form和json

``` js
var express = require('express');
var bodyparser = require('body-parser');

var app = express();

//create application/json parser
var jsonParser = bodyparser.json();

//create application/x-www-form-urlencoded parser
var urlencodedParser = bodyparser.urlencoded({ extended: false})

app.post('/', urlencodedParser,function (req, res) {
  console.dir(req.body);
  res.send(req.body.name);
})

app.post('/upload', jsonParser, function (req, res) {
  console.dir(req.body);
  res.send(req.body.name);
})
```

参考链接：github/express/body-parser

## 六、上传文件

form.html

``` html
<body>
  <form action="/upload" method="post" enctype="multipart/form-data">
    <h2>单图上传</h2>
    <input type="file" name="logo">
    <input type="submit" value="submit">
  </form>
</body>
```

server.js
``` js
var express = require('express');
var bodyparser = require('body-parser');
var fs = require('fs');
var multer = require('multer');
var upload = multer({ dest: 'uploads/'});
var app = express();

//create application/json parser
var jsonParser = bodyparser.json();

//create application/x-www-form-urlencoded parser
var urlencodedParser = bodyparser.urlencoded({ extended: false})

app.get('/', function(req, res) {
  console.dir(req.query);
  res.send('home page:' + req.query.find);
})

app.get('/form', function(req, res) {
  var form = fs.readFileSync('./form.html', { encoding: 'utf8'});
  res.send(form);
})

app.post('/', urlencodedParser,function (req, res) {
  console.dir(req.body);
  res.send(req.body.name);
})

app.post('/upload', upload.single('logo'),function (req, res) {
  res.send({'ret_code': 0});
})

app.listen(3000);
console.log('listening to port 3000');
```
结构变化
![avatar](https://img2018.cnblogs.com/blog/1549437/201901/1549437-20190105205545633-1640494161.png)

参考链接：github/express/multer

tips:

``` js
app.get('/form', function(req, res) {
  var form = fs.readFileSync('./form.html', { encoding: 'utf8'});
  res.send(form);
})
```

等价于
``` js
app.get('/form', function(req, res) {
  res.sendFile(__dirname + '/form.html');
})
```

## 七、模板引擎

引入

app.set('view engine', 'ejs');

使用

server.js
``` js
app.get('/form/:name', function(req, res) {
  var person = req.params.name;
  res.render('form', { person: person});
  res.sendFile(__dirname + '/form.html');
})
```

form.ejs

``` html
<h1><%= person %></h1>
```

遍历数组

``` html
<ul>
  <%= data.hobbie.forEach(function(item){ %>
    <li>
      <%= item %>
    </li>
  <%= }) %>
</ul>
```

公用模板

使用

``` html
<%- include('partials/header.ejs') -%>
```

渲染
``` js
app.get('/about', function(req, res) {
    var data = {age: 29, job: 'programmer', hobbie: ['eating', 'fighting', 'fishing']};
    res.render('about', { data: data});
})
```
参考链接：ejs.co、pug


## 八、中间件

解释：请求与响应之间的处理过程是中间件发挥作用的地方。

好处：共用模块、全局性的操作

类型：
  * 应用级中间件
  * 路由级中间件
  * 错误层次中间件
  * 内置中间件
  * 第三方中间件

执行顺序
``` js
app.use(function(req, res, next){
  console.log('1');
  next();
  console.log('3');
})

app.use(function(req, res, next){
  console.log('2');
})
```

路由中间件

server.js
``` js
var indexRouter = require('./routes/index.js');
var userRouter = require('./routes/user.js');

app.use('/', indexRouter);
app.use('/users', userRouter);
```

./route/index.js

``` js 
var express = require('express');

var router = express.Router();

router.get('/', function(req, res, next) {
  res.send('root');
})

module.exports = router;
```

./route/user.js
``` js
var express = require('express');

var router = express.Router();

router.get('/', function(req, res, next) {
  res.send('user');
})

module.exports = router;
```

体会：用中间件写路由的好处

参考链接：express/using-middleware