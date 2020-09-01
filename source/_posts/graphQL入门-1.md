---
title: graphQL入门-1
date: 2020-06-03 17:08:08
tags: graphQL
categories: 前端
---

## 一、简介
GraphQL是一种新的API标准，它提供了一种更高效、强大和灵活的数据提供方式。
GraphQL是api的查询语言，而不是数据库。
可以在使用API的任何环境中有效使用，我们可以理解为GraphQL是基于API之上的一层封装，目的是为了更好，更灵活的适用于业务的需求变化。

## 二、环境配置
``` bash
npm init
npm install express graphql express-graphql -s
```

## 三、hello world
``` js
const express = require('express');
const {buildSchema} = require('graphql');
const graphqlHTTP = require('express-graphql');

// 定义schema，查询和类型
const schema = buildSchema(`
  type Account {
    name: String
    age: Int
    sex: String
    department: String
  }
  type Query {
    hello: String
    accountName: String
    age: Int
    account: Account
  }
`);

// 定义查询对应的处理器
const root = {
  hello: () => {
    return 'hello world';
  },
  accountName: () => {
    return 'irene';
  },
  age: () => {
    return 18;
  },
  account: () => {
    return {
      name: 'mark',
      age: 18,
      sex: 'male',
      department: 'school'
    }
  }
}

const app = express();

app.use('/graphql', graphqlHTTP({
  schema: schema,
  rootValue: root,
  graphiql: true
}))

app.listen(3000);
```

## 四、使用
用node启动本文件，在localhost:3000/graphql中可以看到运行界面。