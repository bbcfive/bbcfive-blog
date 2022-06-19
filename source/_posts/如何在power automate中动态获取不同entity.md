---
title: 如何在power automate中动态获取不同entity
date: 2022-03-21 14:42:08
tags: Power Platform
categories: Power Platform 
---

#### 一、背景
需要写一个模板flow，从环境变量里获取entity表名，然后在flow中拿到对应的entity内容。
定义了一个entity，display name 和 schema name 分别如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1646991509409-f958e892-3584-44a9-8812-4e3f5b077711.png#clientId=u6a641bbc-8038-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=174&id=u819f5b94&margin=%5Bobject%20Object%5D&name=image.png&originHeight=174&originWidth=330&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7577&status=done&style=none&taskId=ue0bbeebd-3d8b-4ce2-9241-bb75f7c41da&title=&width=330)![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1646991522563-b743d4b6-6e01-4b70-94e4-4f62e3af9434.png#clientId=u6a641bbc-8038-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=137&id=uda33c703&margin=%5Bobject%20Object%5D&name=image.png&originHeight=137&originWidth=295&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5784&status=done&style=none&taskId=u827e2735-62a2-4afe-8528-d0b5b73d338&title=&width=295)
在flow中使用list rows时，需添加table name，一般是从dataverse里选已有的entity(此时entity会自动关联到cds的表)，然后拿到entity内容：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1646994158780-6823c2d5-bef8-469b-b210-f12b013df615.png#clientId=u6a641bbc-8038-4&crop=0.0093&crop=0&crop=1&crop=1&from=paste&height=351&id=u3cdd8099&margin=%5Bobject%20Object%5D&name=image.png&originHeight=354&originWidth=539&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10858&status=done&style=none&taskId=ubc189161-0fb2-4023-a948-2afff0b7cac&title=&width=534)
但同时也可以通过custom ![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1646994431465-9a125ace-3029-4c30-97e4-35f26de5f587.png#clientId=u6a641bbc-8038-4&crop=0&crop=0&crop=0.8382&crop=1&from=paste&height=19&id=u2b240860&margin=%5Bobject%20Object%5D&name=image.png&originHeight=23&originWidth=136&originalType=binary&ratio=1&rotation=0&showTitle=false&size=834&status=done&style=none&taskId=u7673f4b8-a046-4aa1-96a7-2d7a96ebe42&title=&width=114)来手动的传入entity表名，但此时如果手动输入`ProdOnCall`或
`ProdOnCalls`，会报如下错误：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1646994585414-f3feaf52-13c6-4de6-a2e6-edd214e3b941.png#clientId=u6a641bbc-8038-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=428&id=u8c0ee1a5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=428&originWidth=541&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16424&status=done&style=none&taskId=u052b38c9-65b5-4b8c-94ff-82dcd71920d&title=&width=541)
#### 二、调研
猜测是由于手动输入的entity表名并没有被正确的mapping到cds的数据上，因为传入的table name值不对。
经查询，发现正确的table name是`EntitySetName`这个字段。
#### 三、解决

1. 将当前solutions export下来，并解压缩，然后将代码导入vscode；
1. 全局搜索`EntitySetName`；
1. 将对应的字段名传给flow，即可。

例如，当前示例entity的name是：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1646994885370-4deccc42-df8f-43ee-bf45-1291b6bd05b8.png#clientId=u6a641bbc-8038-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=32&id=ub56ef4ce&margin=%5Bobject%20Object%5D&name=image.png&originHeight=32&originWidth=515&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4603&status=done&style=none&taskId=u440b4b8e-cb07-4c84-9b15-0f4c3e150cd&title=&width=515)
于是修改flow即可拿到对应的entity数据了。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1646991722347-617ff675-27c0-43bc-97ce-49fc1a26b249.png#clientId=u6a641bbc-8038-4&crop=0&crop=0&crop=1&crop=0.9187&from=paste&height=480&id=mJ7A2&margin=%5Bobject%20Object%5D&name=image.png&originHeight=480&originWidth=549&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36057&status=done&style=none&taskId=u6e211a5f-44bb-4706-98fb-2d6ff1a3fc8&title=&width=549)
#### 四、总结
在flow中手动输入`Enter Custom Value`时，直接输入当前entity的`schema name`的复数形式即可。

