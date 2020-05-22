---
title: mac文件显示的一个神坑
date: 2019-07-11 16:50:00
tags: mac 
categories: 调试测试
---
## 一、问题
mac os，把A目录下的react工程拷贝到B目录下，运行并使用webpack打包，然后报错“unexpected identifier”

不科学的地方

A目录下运行完全正常，B目录下报错，而B目录下的文件是由A目录拷贝而来...

## 二、解决
一开始当成“unexpected token”来处理（因为病症十分相似），即没转化成es6语法，所以解决方向都是“How To Enable ES6 Imports in Node.JS”

于是添加配置.babelrc
``` json
{
  "presets": ["@babel/preset-env", "@babel/preset-react"],
  "plugins": ["@babel/plugin-proposal-class-properties"]
}
```

才发现一个巨坑：

MAC电脑里，A目录下有此文件（在ide里看得到），但在文件夹（用户图形界面）里没有此文件！！因此拷贝到B目录的工程里没有.babelrc...

检查了系统默认设置，没有发现与此相关的隐藏文件的功能，只能推断是mac系统不显示"."开头的文件...

## 三、显示以'.'开头的文件
在window系统中可以很直观的看到“.”开头的文件
![avatar](https://img2018.cnblogs.com/blog/1549437/201907/1549437-20190711163838018-2105898749.png)

但在mac下需要进行一些额外操作：

#### 方法一：

在终端输入
> defaults write com.apple.finder AppleShowAllFiles TRUE; killall Finder

很明显，若想隐藏，true改为false即可；

#### 方法二：

配置.gitignore 文件

对不想隐藏的文件取反（!）即可，参考http://www.cnblogs.com/haiq/archive/2012/12/26/2833746.html

最终效果：
![avatar](https://img2018.cnblogs.com/blog/1549437/201907/1549437-20190711164522850-924852201.png)

把这些文件增加到B目录后，工程正常运行......