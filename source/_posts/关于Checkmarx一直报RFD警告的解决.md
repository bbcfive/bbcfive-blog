---
title: 关于Checkmarx一直报RFD警告的解决
date: 2022-03-08 10:30:12
tags: 
  - Checkmarx
categories: 调试测试
---

#### 一、背景
需要从当前页面的url里取得params，使用`window.location.search()`拿到参数，本地运行正常。
但在`Checkmarx`进行质量检测的时候被识别为有`Reflected File Download`安全漏洞的代码，无法pass。
#### 二、调研
Reflected File Download是一种网络安全漏洞，它可能使攻击者通过从受信任的域（如 Google.com 和 Bing.com）虚拟下载文件来获得对受害者计算机的完全访问权限。
例如：
> http://www.baidu.com/view?thisisfakeurl.bat

当用户点击这个连接后，浏览器可能会自动解析并下载`.bat`文件到用户电脑，从而造成危险。
#### 三、解决

1. 通过检索关键字，发现一种解决办法是从后端拿参数，避免`window.location`的字眼，但这样过于麻烦；
1. searchParams.get(Key)

通过URL的api拿到query的固定字段来获值并避免RFD风险：
```javascript
new URL(location.href).searchParams.get(yourField)
// Returns 2008 for href = "http://localhost/search.php?year=2008".
// Or in two steps:
const params = new URL(location.href).searchParams;
const year = params.get('year');
```
然后可以顺利通过Checkmarx检测。
