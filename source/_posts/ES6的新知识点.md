---
title: ES6的新知识点
date: 2018-12-13 20:25:12
tags: 
  - JavaScript
  - 基础知识
  - ES6
categories: 前端  
---
## 一、变量
原有变量：

var的缺点：

1.可以重复声明
2.无法限制修改
3.没有块级作用域
新增变量：

let ：不能重复声明，变量-可以修改，块级作用域

const：不能重复声明，变量-不可以修改，块级作用域

## 二、箭头函数
原函数：

var func = function（args）{

　　alert（“abc”）；

}；

现箭头函数：

var func = （args）=> {

　　alert（“abc”）；

}；

简写规则：

（）——只有一个参数

{ } ——只有一个return

所以上述函数还可以写为：

var func = args => alert（“abc”）；

## 三、函数的参数
function（a，b，...args）

...args 可以 替代所有想写的不想写的参数

## 四、解构赋值
1.左右两边结构必须一样

2.右边必须定义

3.声明和赋值不能分开（必须在一句话里完成）

let [a,b,c] = [12,5,8];

let {a,b,c} = {a: 12, b: 5, c: 8};

## 五、数组新操作
map             映射

reduce         汇总

filter             过滤器

forEach       循环（迭代）

## 六、字符串
1.新方法

startsWith
endsWith
2.字符串模板

直接可以把元素嵌套在字符串里   $（元素）
可以折行

## 七、json
1.json对象

JSON.stringify
JSON.parse
2.简写

json = {

　　url : url,

　　show : function(){ }

}

简写为：

json = {

　　url,

　　show(){ }

}

json标准写法：只能用双引号；属性必须用双引号包起来

## 八、promise
有了promise之后的异步：

Promise.all([$.ajax(), $.ajax()]).then(results=>{

　　//success

}, err=>{

　　//error

});

## 九、generator
可以暂停函数的函数

//创建generator函数

function *func（） {

　　//do sth A

　　yield;

　　//do sth B

}

//调用

let genObj = func();

genObg.next(); //do A

genObg.next(); //do B

 yield ：可以传参，也可以返回
异步操作：

1.回调

2.promise

3.generator（runner.js）

runner( function *() {

　　let data1 = yield $.ajax({url: xxx, dataType : 'json'});

　　let data2 = yield $.ajax({url: xxx, dataType : 'json'});

　　let data3 = yield $.ajax({url: xxx, dataType : 'json'});

});

Promise——适合做没有逻辑的异步

generator——适合做逻辑性异步