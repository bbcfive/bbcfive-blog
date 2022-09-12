---
title: msg.sender和tx.origin的区别
date: 2022-09-12 11:32:12
tags: 
  - 区块链
  - Web3
  - Solidity
categories: 区块链
---

#### 一、简介
- msg.sender
> msg.sender (address): sender of the message (current call)
指直接调用智能合约函数的账户地址或智能合约地址。

- tx.origin
> tx.origin (address): sender of the transaction (full call chain)
指调用智能合约函数的账户地址，只有账户地址可以是tx.origin。

![avatar](../../../../images/msgtx.png)

您可能会注意到，账户地址和智能合约地址都可以是 msg.sender 但 tx.origin 将始终是账户/钱包地址。

强烈建议始终使用 msg.sender 进行授权或检查调用智能合约的地址。 并且永远不要使用 tx.origin 进行授权，因为这可能会使合约容易受到网络钓鱼攻击。

另外，一些NFT项目会使用`msg.sender == tx.origin`来限制科学家使用脚本来刷白名单，以此确保每一个交易来自真实user而非脚本合约。

本文翻译自：[https://davidkathoh.medium.com/tx-origin-vs-msg-sender-93db7f234cb9](https://davidkathoh.medium.com/tx-origin-vs-msg-sender-93db7f234cb9)