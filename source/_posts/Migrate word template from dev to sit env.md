---
title: How to migrate word template from dev to sit/uat env
date: 2022-03-09 16:00:08
tags: Power Platform
categories: Power Platform 
---

#### 一、背景
在dev环境下创建的Word template关联dev的entity之后，将dev的solutions打包并迁移到sit环境时，发现Word template无法正常加载了，报错：`can't find the entity`。
#### 二、调研
经查，同一个entity在不同的环境下对应的`**ObjectTypeCode**`不同，因此旧的Word template导入新环境后，找不到之前的object type code，因此报错。
#### 三、解决
给Word template添加后缀`.zip`：
[![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1646812497601-4354c5a8-4d3c-44a3-b467-3309eb965fac.png#clientId=ub9461f11-725a-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u8dd8addc&margin=%5Bobject%20Object%5D&name=image.png&originHeight=48&originWidth=489&originalType=url&ratio=1&rotation=0&showTitle=false&size=9683&status=done&style=none&taskId=ub2ba3002-83c1-4650-87c6-7c24c126624&title=)](https://i0.wp.com/debajmecrm.com/wp-content/uploads/2019/01/image-5.png?ssl=1)
因为是`.zip` 文件, 因此可以解压缩它得到以下文件：
[![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1646812497567-4191aba5-ede8-46eb-bad4-91293c7a8b89.png#clientId=ub9461f11-725a-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ueba1ac54&margin=%5Bobject%20Object%5D&name=image.png&originHeight=112&originWidth=403&originalType=url&ratio=1&rotation=0&showTitle=false&size=18563&status=done&style=none&taskId=u2837eb45-6a88-4335-a2d2-2deba182723&title=)](https://i0.wp.com/debajmecrm.com/wp-content/uploads/2019/01/image-6.png?ssl=1)
在当前目录下搜索object type code，这里是10426：
[![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1646812497617-766cd339-a000-4c1b-a6d6-e57f754dc8f8.png#clientId=ub9461f11-725a-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ue653362c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=100&originWidth=493&originalType=url&ratio=1&rotation=0&showTitle=false&size=29687&status=done&style=none&taskId=u063c11ad-953a-44cd-9cf6-413ad067833&title=)](https://i0.wp.com/debajmecrm.com/wp-content/uploads/2019/01/image-7.png?ssl=1)
然后在vscode或其他工具里打开它，并逐一修改object type code至新环境下entity的object type code：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1646812747410-eb94307c-9c62-4fdd-b3a9-0a3ee904eb94.png#clientId=ub9461f11-725a-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=416&id=u77d25fd5&margin=%5Bobject%20Object%5D&name=image.png&originHeight=516&originWidth=334&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41164&status=done&style=none&taskId=ubdc76ae5-5af0-440c-898d-79f9714f939&title=&width=269)
然后保存，重新打包成`.docx`的Word文档：
[![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1646812497532-af6557e3-10a5-4968-8c58-7844fd1e208b.png#clientId=ub9461f11-725a-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u5d30082a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=197&originWidth=437&originalType=url&ratio=1&rotation=0&showTitle=false&size=27544&status=done&style=none&taskId=u94a09708-f413-4fbe-9a3e-5a7d2ec9baa&title=)](https://i0.wp.com/debajmecrm.com/wp-content/uploads/2019/01/image-8.png?ssl=1)
现在将其导入到新环境的Word模板文件时就不会报错了。

参考：

1. [Migrate document templates across environments in Dynamics 365](https://debajmecrm.com/how-do-i-move-my-word-template-for-custom-entity-across-environments-in-dynamics-365-how-can-i-make-the-changes-manually-a-deep-dive-into-inner-workings/)

