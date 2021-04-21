---
title: 搜索引擎中的TF-IDF
date: 2021-04-08 14:29:08
tags: 
  - 深度学习
  - NLP
categories: 机器学习
---
本文是对[统计学让搜索速度起飞](https://mofanpy.com/tutorials/machine-learning/nlp/tfidf/)一文的学习总结。

## 一、搜索引擎中的TF-IDF
  TF-IDF全称是Term Frequency - Inverse Documents Frequency，代表词频和逆文档频率指数关系。它量化出词语的重要程度和独特性，从而标记文章，再通过对比搜索关键词与文章标记的相似性，从而提供最相似的文章列表。
  TF-IDF是一种基于统计学的方法，而非机器学习，因为深度学习在处理单个文档的速度过慢，相比之下统计学拥有更快的速度。

## 二、什么是TF-IDF
  IF是指一个词在文档中出现的频率，一般来说出现频率越高的词，越能作为本篇文章的关键词，但也有特殊情况，例如文章中大量出现的“我”， “本文”等无意义的词，虽然其词频（TF）很高，但区分度（IDF）不大。于是我们把局部信息TF和全局信息IDF结合在一起分析的时候，就能得到更准确的结论。

## 三、TF-IDF的量化过程
  TF-IDF的本质就是将词语重要程度转换成向量并进行量化计算的操作，假设现有三个词语：“WordA”、“WordB”、“WordC”和三个文档“Doc1”，“Doc2”，“Doc3”，我们可以用TF-IDF表达每个word在doc里出现的具体次数，从而将文档量化成一个三维向量：

vector | WordA | WordB | WordC
:-: | :-: | :-: | :-:
Doc1 | x1 = 1 | y1 = 13 | z1 = 7
Doc2 | x2 = 2 | y2 = 12 | z2 = 6
Doc3 | x3 = 3 | y3 = 11 | z3 = 5

当我们把向量展示在三维坐标系中，两个空间向量之间的夹角，比如cosine的距离，就是文档的相似度。

## 四、实现
代码参考：https://github.com/bbcfive/Python-project/blob/master/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AF%87/NLP/tf-idf.py






