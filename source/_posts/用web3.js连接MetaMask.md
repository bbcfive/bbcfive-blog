---
title: 连接webApp到MetaMask钱包
date: 2021-04-20 14:20:08
tags: 
  - 智能合约
  - 以太坊
categories: 区块链
---

## 一、基本概念
  MetaMask是基于ETH链的钱包，web3.js是一个前端库。前端页面可以通过web3.js来将web app连接MetaMask钱包（一般是以一个Chrome插件的形式存在于浏览器中），从而进行交易。

## 二、具体实现
  ``` javascript
  import Web3 from "web3";

  const onSubmit = (values) => {
    if (window.ethereum?.isMetaMask) {
      window.web3 = new Web3(window.ethereum);
      window.ethereum.enable();
    } else {
      alert('请先下载Chrome应用商店内下载MetaMask!');
    }
  };
  ```
  然后将onSubmit函数绑定到页面中的一个button上，当点击button时，如果检测到Chrome中已安装MetaMask插件，则会直接调出钱包并自动连接：
  ![avatar](https://img2020.cnblogs.com/blog/1549437/202104/1549437-20210421144245932-535278272.png)








