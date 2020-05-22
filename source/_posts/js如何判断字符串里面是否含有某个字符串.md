---
title: js如何判断字符串里面是否含有某个字符串
date: 2018-11-29 22:34:08
tags: 基础知识
categories: 前端
---



方法一: indexOf() (推荐)
``` js
var str = "123";
console.log(str.indexOf("3") != -1 ); // true
```
indexOf() 方法可返回某个指定的字符串值在字符串中首次出现的位置。如果要检索的字符串值没有出现，则该方法返回 -1。

方法二: search()
``` js
var str = "123";
console.log(str.search("3") != -1 ); // true
```
search() 方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。如果没有找到任何匹配的子串，则返回 -1。

方法三:match()
``` js
var str = "123";var reg = RegExp(/3/);if(str.match(reg)){ // 包含 }
```
match() 方法可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。