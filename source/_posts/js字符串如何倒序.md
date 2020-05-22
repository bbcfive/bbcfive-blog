---
title: js字符串如何倒序
date: 2018-11-29 22:32:08
tags: 基础知识
categories: 前端
---

1.

``` js
var reverse = function( str ){
var newStr = '', i = str.length;
for(; i >= 0; i--) {
newStr += str.charAt(i);
}
return newStr;
};

reverse('abcde')
```
 

2.

``` js
var reverse = function( str ){
return str.split('').reverse().join('');
};
```

3.（类似法2）

``` js
var reverse = function( str ){
var stack = [];//生成一个栈
for(var len = str.length,i=len;i>=0;i-- ){
stack.push(str[i]);
}
return stack.join('');
};
```
 