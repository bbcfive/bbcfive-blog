---
title: 三种角度解释href/src/link/import区别
date: 2018-12-04 16:42:12
tags: 
  - JavaScript
  - 基础知识
categories: 前端  
---

解释一：

href是Hypertext Reference的缩写，表示超文本引用。用来建立当前元素和文档之间的链接。常用的有：link、a。例如：
``` html
<link href="reset.css" rel=”stylesheet“/>
```

浏览器会识别该文档为css文档，并行下载该文档，并且不会停止对当前文档的处理。这也是建议使用link，而不采用@import加载css的原因。

src是source的缩写，src的内容是页面必不可少的一部分，是引入。src指向的内容会嵌入到文档中当前标签所在的位置。常用的有：img、script、iframe。例如
``` html
<script src="script.js"></script>
```

当浏览器解析到该元素时，会暂停浏览器的渲染，直到该资源加载完毕。这也是将js脚本放在底部而不是头部得原因。

解释二：

href标识超文本引用，用在link和a等元素上，href是引用和页面关联，是在当前元素和引用资源之间建立联系

src表示引用资源，表示替换当前元素，用在img，script，iframe上，src是页面内容不可缺少的一部分。src是source的缩写，是指向外部资源的位置，指向的内部会迁入到文档中当前标签所在的位置；在请求src资源时会将其指向的资源下载并应用到当前文档中，例如js脚本，img图片和frame等元素。

补充：link和@import的区别

两者都是外部引用CSS的方式，但是存在一定的区别：

区别1：link是XHTML标签，除了加载CSS外，还可以定义RSS等其他事务；@import属于CSS范畴，只能加载CSS。

区别2：link引用CSS时，在页面载入时同时加载；@import需要页面网页完全载入以后加载。

区别3：link是XHTML标签，无兼容问题；@import是在CSS2.1提出的，低版本的浏览器不支持。

区别4：link支持使用Javascript控制DOM去改变样式；而@import不支持。

解释三：

href 表示超文本引用（hypertext reference），在 link和a 等元素上使用。src 表示来源地址，在 img、script、iframe 等元素上。
src 的内容，是页面必不可少的一部分，是引入。href 的内容，是与该页面有关联，是引用。区别就是，引用和引入。