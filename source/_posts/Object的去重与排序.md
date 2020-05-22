---
title: Object的去重与排序
date: 2020-01-20 19:58:09
tags: 
  - 排序
  - 去重
  - object
categories: Algorithm  
---
## 要求
有N个1到1000之间的随机整数（N≤1000），对于其中重复的数字，只保留一个，把其余相同的数去掉，然后再把这些数从小到大排序输出。

## 形式
``` js
Input Param

n               输入随机数的个数

inputArray      n个随机整数组成的数组


Return Value

OutputArray    输出处理后的随机整数
```

## 解决
JavaScript：
``` js
while(n=parseInt(readline())){
    var arr=[];
    while(n--){
        var value= parseInt(readline())
        arr[value]=1; // typeOf(arr) = object,其中value键按照数字由小到大的顺序排列的，因此不用再次sort，同时也兼具了去重功能
    }
    arr.forEach((el,index)=>{if(el===1){console.log(index)}})
}
```
Python
``` python
while True:
    try: // 检测输入行是否还有输入，没有的话终止就立刻循环，转到except后break出去
 
        a,res=int(input()),set() // 直接用set()排序
        for i in range(a):res.add(int(input()))
        for i in sorted(res):print(i)
 
 
    except:
        break
```