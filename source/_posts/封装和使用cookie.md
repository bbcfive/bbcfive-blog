---
title: 封装和使用cookie（内含彩蛋）
date: 2018-12-09 09:24:12
tags: 
  - JavaScript
  - 基础知识
categories: 前端  
---

## 一、什么是cookie？

页面用来保存信息，如：自动登录、记住用户名

## 二、cookie的特性
1. 同一个网站中所有页面共享一套cookie
2. 数量、大小有限
3. 有过期时间

## 三、js中使用cookie
document.cookie

## 四、cookie的使用
1. 设置cookie：
格式：名字=值（多条不会覆盖）
过期时间：expires = 时间
封装函数
2. 读取cookie（字符串的分割）
3. 删除cookie（已经过期）

## 五、封装cookie
``` js
//创建cookie
function setCookie(name,value,expiresDay) {
  var oDay = new Date();
  oDay.setDate(oDay.getDate() + expiresDay);

  document.cookie = name + ' = ' + value + '; expires = ' + expiresDay;
}

//得到cookie
function getCookie(name) {
  var arr = document.cookie.split('; '); //cookie间是用;+空格隔开的
  for (let i = 0; i < arr.length; i++) {
    var arr2 = arr[i].split('=');
    if (arr2[0] == name) {
      return arr2[1];
    }
  }
  return '';
}

//删除cookie
function removeCookie(name) {
  setCookie(name,1,-1); //-1天后过期，则浏览器立马删除
}

setCookie('userName','blue',100);
```

## 六、cookie简单示例（网页登录中应用cookie）
JavaScript：
``` js
window.onload = function() {
  var oForm = document.getElementById('form1');
  var oUser = document.getElementsByName('user')[0];

  oForm.onsubmit = function() {
    setCookie('user', oUser.value, 14);
  }

  oUser.value = getCookie('user');
}
```

html：
``` html
<form id="form1" action="http://www.baidu.com/">
    用户名:<input type="text" name="user"> <br>
    密码:<input type="password" name="password"> <br>
    <input type="submit" value="登录">
</form>
```

## 七、彩蛋

表单提交到了百度的服务器，于是在百度首页的console里看到如下文字：

![avatar](https://img2018.cnblogs.com/blog/1549437/201812/1549437-20181209092717853-1388520748.png)

嗯，好大一个蛋~