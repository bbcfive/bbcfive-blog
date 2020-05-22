---
title: RN项目里引用moment库闪退
date: 2019-06-17 14:51:10
tags: 
  - JavaScript 
  - React Native
categories: 调试测试
---
在RN项目里引用moment库，使用了如下一句：

var timeNow = moment().<font color="#FF4500">locale('zh-cn')</font>.format('YYYY-MM-DD HH:mm:ss');

结果，调试环境下正常，没有报错，但打包后app运行闪退。。。

去掉.locale('zh-cn')后再打包一切正常。。。

不知道是moment库与手机系统哪里冲突。。。真的坑。。。

 