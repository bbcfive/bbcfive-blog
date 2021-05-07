---
title: js creates a random tree
date: 2020-06-02 22:34:08
tags: tree
categories: Algorithm
---

## Q：
完成函数genData(count)，它返回count条树结构的节点数据。形如：
![avatar](https://img2020.cnblogs.com/blog/1549437/202006/1549437-20200603180755992-2016120284.png)

## A：
``` js
// generate name
function genName(nameLen) {
  var nameStr = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  var name = '';
  for(var i = 0; i < nameLen; i++) {
    // get 0-25
    var rand = Math.floor(Math.random()*26); 
    name+=nameStr[rand];
  }
  return name;
}

// swap elements in arr
function swapArr(arr, i1, i2) {
  arr[i1] = arr.splice(i2, 1, arr[i1])[0];
  return arr;
}

function genData(count) {
  var objArr = [], obj = {}, rand = 0;
  var rootName = genName(5);
  var rootObj = {id: 0, name: rootName, parent: null};
  objArr.push(rootObj);                 
  for (var c = 1; c < count; c++) {
    var parentNum = Math.floor(Math.random()*c);
    obj = {id: c, name: genName(5), parent: parentNum}
    objArr.push(obj);
  }
  // iteration function, make objArr listed by parentNum from low to high
  for(var m = 0; m < objArr.length - 1; m++) {
    for (var n = 0; n < objArr.length - m - 1; n ++) {
      if (objArr[n].parent > objArr[n + 1].parent ) {
        swapArr(objArr, n, n + 1);
      } 
    }
  }
  return objArr;
}
```
