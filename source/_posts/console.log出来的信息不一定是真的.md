---
title: console.log出来的信息不一定是真的
date: 2019-03-10 19:50:12
tags: react-native
categories: 调试测试  
---

## 一、问题

拿接口取值，明明this.props.chartsValue[0]已经返回json数据，结果this.props.chartsValue[0].history报错：无法获得undefined数据的history属性！...

## 二、原因

数据操作，尤其是取值操作是有时间过程的。console.log出来的信息不代表是此刻变量的数据值（可能是在数据传进来之前就log了），除非一直console.log，才能看到信息从无到有的过程。

## 三、解决

做数据判断处理，确保在准确拿到值以后，再进行相关操作。

``` js
const obj = this.props.chartsValue;
  var time = [], value = [];      
  if(obj&&obj[0]) {  //数据判断处理
  var arr = obj[0].history;
  arr.map((item, i) => {
    time.push(item.time);
    value.push(item.value);
  });
  console.log(value);                  
}
```

# 进阶

## 一、问题

已做数据判断处理，但无log信息显示：
``` js
if (storage.cache.user_1001) {
  console.log(storage.cache.user_1001);
}
```

## 二、原因

数据的加载过程需要时间，而外层的事件是单次触发事件（即，没有持续的监听数据变化），导致执行到数据判断时，判断没有信息，故不执行log语句。

## 三、解决

延时异步执行（async action），成功获取数据。

setTimeout(() => {
  console.log(storage.cache.user_1001);
}, 500);     


附：async 函数的使用（http://es6.ruanyifeng.com/#docs/async）