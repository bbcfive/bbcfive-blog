---
title: Javascript中的call详解
date: 2019-03-27 13:46:08
tags: 
  - JavaScript
  - 基础知识
categories: 前端
---

## 一、call的使用

call 方法第一个参数是作为函数上下文的对象，第二个参数是一个参数列表。

``` js
var obj = {
  name: 'J'
}

function func(p1, p2) {
  console.log(p1+ ' ' + p2 + ' ' + this.name);
}

func.call(obj, 'I', 'am');       // I am J
```

## 二、call的作用

1.改变 this 指向

``` js
var obj = {
  name: 'J'
}

function func() {
  console.log(this.name);
}

func.call(obj);       // J
```

2.借用别的对象的方法
``` js
var Person1  = function () {
  this.name = 'J';
}
var Person2 = function () {
  this.getname = function () {
    console.log(this.name);
  }
  Person1.call(this);
}
var person = new Person2();
person.getname();       // J
```

3.调用函数
``` js
function func() {
  console.log('J');
}
func.call();     // J
```

参考文献：[https://github.com/lin-xin/blog/issues/7](https://github.com/lin-xin/blog/issues/7)