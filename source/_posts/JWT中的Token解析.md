---
title: JWT中Token的解析
date: 2020-12-02 09:55:00
tags: 基础原理 
categories: 前端
---

## JWT中Token的解析
本文是对[JWT生成Token做登录校验](https://blog.csdn.net/sky_jiangcheng/article/details/80546370)一文的阅读总结。

## 一、简介
JWT全称json web token，一般被用来在身份提供者和服务提供者之间传递被认证的用户身份信息，以便在服务器上获取资源。比如登陆验证。

## 二、传统的登陆认证————session+cookie
在传统的登陆认证时，由于http是无状态的，所以一般基于session方式，当用户登录成功，服务器会保证一个session，并返还给客户端一个sessionId，客户端将seesionId存在cookie中，每次请求都会携带sessionId。
![avatar](http://p3.pstatp.com/large/50af0002c8ac4684ab71)

这种方式的缺点是：
1. cookie+session这种模式通常是保存在内存中，而且服务从单服务到多服务会面临的session共享问题；
2. 随着用户量的增多，开销会越大。

## 三、JWT认证————token
反观JWT，只要服务端生成token，客户端保存这个token，每次请求携带这个token，服务端认证解析即可。
![avatar](http://p1.pstatp.com/large/53e5000277b955a33495)


## 四、拆解JWT
1. JWT生成Token后的样子
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJvcmciOiLku4rml6XlpLTmnaEiLCJuYW1lIjoiRnJlZeeggeWGnCIsImV4cCI6MTUxNDM1NjEwMywiaWF0IjoxNTE0MzU2MDQzLCJhZ2UiOiIyOCJ9.49UF72vSkj-sA4aHHiYN5eoZ9Nb4w5Vb45PsLF7x_NY

2. JWT的构成
    ```
    HEADER: ALGORITHM & TOKEN TYPE
    {
      "alg": "RS256", // 声明加密的算法
      "typ": "JWT" // 声明类型
    }
    PAYLOAD: DATA
    {
      "jti": "AT.By_jMS74u-RCcO05ZsY9wnoSyH5D6kHXSkGDZ4c", // jwt的唯一身份标识，主要用来作为一次性token,从而回避重放攻击
      "iss": "https://yourorganizition/oauth2/", // jwt签发者
      "aud": "api://dafault", // 接收jwt的auth服务器
      "iat": 1606872614, // jwt的签发时间
      "exp": 1606876214, // jwt的过期时间
      "cid": "0oac4zNMOvkYC0", // clientId
      "uid": "00uukg2dEf1Ws0", // userId
      "sub": "bbcfive@163.com" // jwt所面向的用户
    }
    VERIFY SIGNATURE
    RSASHA256(
      base64UrlEncode(header) + "." + // base64加密后的header
      base64UrlEncode(payload), // base64加密后的payload
      your-256-bit-PUBLIC-KEY // 密钥secret是保存在服务端的，服务端会根据这个密钥进行生成token和验证
    )
    ```
  
## 五、JWT小结
1. 因为json的通用性，所以JWT是可以进行跨语言支持的，像JAVA,JavaScript,NodeJS,PHP等很多语言都可以使用。
2. payload部分，JWT可以往里加自定义字段。
3. 便于传输，jwt的构成非常简单，字节占用很小，所以它是非常便于传输的。它不需要在服务端保存会话信息, 所以它易于应用的扩展。

