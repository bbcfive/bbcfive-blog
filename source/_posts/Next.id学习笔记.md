---
title: 使用Next.id做Web3Auth
date: 2022-06-15 23:30:12
tags: 
  - Web3
  - 区块链
  - 以太坊
categories: 区块链
---

## Next.id是什么
[Next.id](https://docs.next.id/)是一个可以连接Web2和Web3应用并用来做auth认证的Web3基础设施工具。其主要功能分为三个模块：
 - [proof-service](https://docs.next.id/proof-service/ps-intro): 提供了 DID(去中心化ID)服务，用于在非对称密码学（目前为椭圆曲线）和其他身份提供者（其他非对称密码学 ID、Web2.0 身份提供者等）之间建立连接。
 - [kv-intro](https://docs.next.id/kv-service/kv-intro): 旨在以可追溯和去中心化的方式保存/读取用户数据。
 - [relation-service](https://docs.next.id/relation-service/intro)


## proof-service应用
proof-service主要有两种应用：
1. 跟第三方Web2平台建立链接
2. 跟Ethereum建立链接

## 与Twitter建立链接
流程如下：
![avatar](../../../../images/NextIdLinkCreationFlow.jpg)
### 1. Dapp发送post请求给ProofService
![avatar](../../../../images/NextIdPost1.jpg)
注意📢：
 - identity是Twitter的账号id
 - public_key是以太坊账号地址的私钥(可以通过metask钱包导出)所产生的公钥(可以通过[ethereum-private-key-to-public-key](https://lab.miguelmota.com/ethereum-private-key-to-public-key/example/)点击获取)

### 2. 拿到返回的sign_payload和post_content，用sign_payload在metamask进行签名，拿到signature
代码：
```typescript
import Web3 from 'web3';

try {
  let web3: Web3 | undefined = undefined; // Will hold the web3 instance
  const signature: any = await web3?.eth.personal.sign(
    "{\"action\":\"create\",\"created_at\":\"1654708182\",\"identity\":\"irenefive\",\"platform\":\"twitter\",\"prev\":null,\"uuid\":\"e123879b-fa29-4fb7-9aac-b57137f079a1\"}",
    publicAddress,
    '' // MetaMask will ignore the password argument here
  );
  console.log(web3)
  console.log(signature)
  return { publicAddress, signature };
} catch (err) {
  throw new Error(
    'You need to sign the message to be able to log in.'
  );
}
```
  运行界面：
  ![avatar](../../../../images/NextIdSignReq.jpg)
  注意📢：
   - 如果你的signature是64位的，需要使用node自带的Buffer.from(str, "hex").toString("base64")转成Base64格式的string
   - 注意传入Buffer.from的第一个参数str如果是16进制的，不需要带0x，即把"0xabc..."替换为"abc..."即可
  ![avatar](../../../../images/BufferToBase64.jpg)
 

### 3. 去社交平台上发布带有signature(Base64格式)的post_content
![avatar](../../../../images/TwitterSig.jpg)
注意📢：
Sig:和签名直接需要加一个空格，否则签名将不被识别。

### 4. Dapp再次发送post请求给ProofService
![avatar](../../../../images/NextIdPost2.jpg)
可以看到Status：201，即为创建成功
注意📢：
 - proof_location即为Twitter的推文ID，例如：
 > https://twitter.com/IreneFive/status/1536727305883910144
 ID为`1536727305883910144`
 - uuid和created_at都为上一个post请求返回值里的数据

### 5.使用get /v1/proof查询创建记录
![avatar](../../../../images/NextIdGet1.jpg)
此时已经可以通过接口查询到link的创建记录，说明与Twitter的链接成功。








