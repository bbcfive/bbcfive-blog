---
title: Typescript基础
date: 2020-06-05 14:38:01
tags: TypeScript
categories: 前端
---

## 一、数据类型
1. 布尔类型(boolean)
  var flag:boolean = true
2. 数字类型(number)
  var a:number = 123
3. 字符串类型(string)
  var string = "this is ts"
4. 数组类型(array)
  其中数组的定义方式有两种：
  方法一
    let arr:number[] = [1,2,3,4]
    let arr:string[] = ["php", "js", "golang"]

  方法二
    let arr:Array<number> = [1,2,3,4]
    let arr:Array<string> = ["php", "js", "golang"]
5. 元组类型(tuple) (属于数组的一种
  let arr:[string,number,boolean] = ["ts",3.18,true]
6. 枚举类型(enum) 用于标识状态的固定值，提升可读性
  ``` js
  enum Flag { success = 1, error = -1}
  var f:Flag = Flag.success
  ```
7. 任意类型(any)
  var num:any = 123;
8. null和undefined 其他数据类型的子类型
  var num:undefined;
  var num:number | undefined;  
  var num:number | undefined | null;  
9. void类型 用于定义方法的时候方法没有返回值
  ``` js
  function run():viod {
    console.log('run');
  }
  // 反之
  function run():number {
    return 123;
  }  
  ```
10. never类型 
指其他类型(包括null和undefined)，代表从不会出现的值
声明never类型的变量只能被never类型所赋值

## 二、函数的定义
1. 一般定义
  function run():number {
    return 123;
  }  

2. 传参定义
  ``` js
  function getInfo(name:string, age:number):string {
    // 模板字符串
    return `${name} --- ${age}`;
  }

  // 匿名函数
  var getInfo = function(name:string, age:number):string {
    // 模板字符串
    return `${name} --- ${age}`;
  }
  ```
3. 可选参数
  ``` js
  function getInfo(name:string, age?:number):string {
    if(age) {
      return `${name} --- ${age}`;
    } else {
      return `${name} --- 年龄保密`;
    }
  }
  ```

4. 默认参数
  ``` js
  function getInfo(name:string, age:number = 20):string {
    return `${name} --- ${age}`;
  }
  ```

5. 剩余参数
  ``` js
  function sum(a:number, b:number, c:number, d:number):number {
    return a+b+c+d;
  }

  // 三点运算符接受形参传过来的值
  function sum(...result:number[]):number {
    var sum = 0;
    for(var i = 0; i < result.length; i++) {
      sum += result[i];
    }
    return sum;
  }

  function sum(a:number, ...result:number[]):number {
    var sum = a;
    for(var i = 0; i < result.length; i++) {
      sum += result[i];
    }
    return sum;
  }
  ```

6. 函数重载
  ``` js
  // Java中的重载：指两个或两个以上同名函数，但它们的参数不一样，这时会出现函数重载情况。
  // typescript中的重载：通过为同一个函数提供多个函数  类型定义来试下多种功能的目的。

  function getInfo(name:string):string;
  function getInfo(age:number):number;
  function getInfo(str:any):any {
    if (typeOf str === "string") {
      return '我叫' + str;
    } else {
      return '我的年龄是' + str;
    }
  }  
  ```