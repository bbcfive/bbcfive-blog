---
title: 存放数据至去中心化服务器IPFS上
date: 2022-02-23 16:37:12
tags: 
  - 区块链
  - Web3
  - 去中心化存储
categories: 区块链
---

#### 一、IPFS简介
IPFS(InterPlanetary File System)是一个基于[libp2p](https://libp2p.io/)库的去中心化服务器，遵循P2P协议。

#### 二、前端使用IPFS

1. 安装依赖
> yarn add ipfs-core

2. 在前端文件里引入IPFS
> import * as IPFS from 'ipfs-core';

3. 写数据到IPFS
```javascript
const ipfs = await IPFS.create()
const { cid } = await ipfs.add("Stay hungry, stay foolish")

console.info(cid.toString()) 
// QmTxPaRKovztC9assAEas69CMTxagzPuYBq2TirSHYCjz9
```

4. 从IPFS中读取数据
```javascript
const stream = ipfs.cat(cid.toString())
let data = ''

for await (const chunk of stream) {
  // chunks of data are returned as a Buffer, convert it back to a string
  data += chunk.toString()
}

console.log(data)
// "Stay hungry, stay foolish"
```

5. 结果预览

![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1645603992240-de97c86d-e7f2-485b-80c8-f96baa994098.png#clientId=ub1f077f9-bad7-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=42&id=u74d93570&margin=%5Bobject%20Object%5D&name=image.png&originHeight=42&originWidth=212&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1320&status=done&style=none&taskId=ubb672380-d359-4842-b2c2-35be0051da6&title=&width=212)
通过http查看：[https://ipfs.io/ipfs/QmTxPaRKovztC9assAEas69CMTxagzPuYBq2TirSHYCjz9](https://ipfs.io/ipfs/QmTxPaRKovztC9assAEas69CMTxagzPuYBq2TirSHYCjz9)

6. 存储数组等其他数据格式

写：
> ipfs.add(JSON.stringify(mockArray))

读：
> console.log(JSON.parse(data))

7. 关于地址

一个地址只对应一个数据，例如`QmTxPaRKovztC9assAEas69CMTxagzPuYBq2TirSHYCjz9`这个地址对应的就是`Stay hungry, stay foolish`这句话，如果更改了这句话的内容，比如`Hello world!!!`, ipfs将会创建一个新的地址用来存放新的数据。
所以对于原先不用的数据，如果未及时删除，就会被永久存储在ipfs上的某个地址，造成浪费。

8. 其他

IPFS官网：[https://js.ipfs.io/zh-CN/](https://js.ipfs.io/zh-CN/)
IPFS支持的操作： [https://github.com/ipfs/js-ipfs/pull/107](https://github.com/ipfs/js-ipfs/pull/107)
IPFS Core Api：[https://github.com/ipfs/js-ipfs/tree/master/docs/core-api](https://github.com/ipfs/js-ipfs/tree/master/docs/core-api)


