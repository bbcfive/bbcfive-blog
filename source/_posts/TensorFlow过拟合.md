---
title: 函数过拟合
date: 2018-06-24 11:55:12
tags: 
  - python
  - TensorFlow
categories: 机器学习  
---

## 一、什么是过拟合
简单来说就是机器学习过于追求训练误差小而导致函数的实际误差过大、泛化能力差问题。

## 二、如何避免过拟合
1. L1，L2..regularization
y = Wx
L1: cost = (Wx - real y)^2 + abs(W)
L1: cost = (Wx - real y)^2 + abs(W)^2

这样一来机器学习的训练误差不仅要考虑数据的弥合度(Wx - real y)，还需要加上数据的偏移度(abs(W))，可以起到防止过拟合的效果。

2. Dropout regularization
随机的丢掉一些神经元，提高函数预测的泛化能力。

本文是对[周沫凡](https://morvanzhou.github.io/tutorials/)同学tf课程的学习笔记记录。