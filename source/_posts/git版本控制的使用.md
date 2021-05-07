---
title: git版本控制的使用
date: 2019-01-03 18:32:08
tags: git
categories: 前端
---

特别说明：本文所有知识笔记是学习“表严肃”同学的git课程记录所得。

前辈个人网站地址：http://biaoyansu.com

特此感谢前辈！

## 一、git是版本控制利器

## 二、本地创建仓库

1.进入新建文件夹test1，打开terminal

git init 

2.直接打开terminal

git init test2

3.直接去网页上拷贝链接，然后打开terminal

git clone + 链接

## 三、基本用法

git status 查看仓库状态

git add . （当前目录下所有的文件全部）增加至.暂存区

git commit -m "XXX" 提交并给它添加说明“XXX”

git log 查看版本历史记录  git log -p 查看详细的修改内容  git log --online 显示在一行  git log --online --all显示所有节点  git log --all --graph 图示全部历史记录

git checkout XXX 穿越到指定的历史节点（回溯到指定节点，head标签定位于此）

touch a.txt 新建文本文件

## 四、三种状态

modified（已修改）=》staged（已暂存）=》commited（已提交）

![avatar](https://img2018.cnblogs.com/blog/1549437/201901/1549437-20190104090448302-678927003.jpg)

## 五、标签tag

git tag -a 标签名 -m “备注” XXX（历史节点）  git tag 列出所有标签

git show 标签名 查看某个标签的详细信息

## 六、分支branch

git branch 分支名 创建分支

![avatar](https://img2018.cnblogs.com/blog/1549437/201901/1549437-20190104091205178-315894337.jpg)

## 七、合并分支

git checkout -b 分支名 创建并切换至分支

git merge 分支名 合并此分支至当前分支中

若两分支内容有冲突，可以手动修改并更新

## 八、远程仓库

git remote add 远程名称 远程地址  添加远程仓库

git remote -v 列出所有远程仓库详细信息

git push -u 远程名 分支名 上传代码

git clone 仓库地址 拷贝仓库

## 九、多人远程合作

git pull 获取远程更新

上述操作等同于 git fetch && git merge

![avatar](https://img2018.cnblogs.com/blog/1549437/201901/1549437-20190104094953592-1804855720.png)

附录：

（基操一览表）

![avatar](https://img2018.cnblogs.com/blog/1549437/201901/1549437-20190104095632840-598750135.png)