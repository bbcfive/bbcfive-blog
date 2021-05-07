---
title: graphQL-middleware
date: 2020-06-03 17:38:08
tags: graphQL
categories: 前端
---

## 使用中间件进行权限的控制

``` js
const express = require('express');
const {buildSchema} = require('graphql');
const graphqlHTTP = require('express-graphql');

// 定义schema，查询和类型
const schema = buildSchema(`
  input AccountInput {
    name: String
    age: Int
    sex: String
    department: String
  }
  type Account {
    name: String
    age: Int
    sex: String
    department: String
  }
  type Mutation {
    createAccount(input: AccountInput): Account
    updateAccount(id: ID!, input: AccountInput): Account
  }
  type Query {
    accounts: [Account]
  }
`);

const fakeDb = {};

// 定义查询对应的处理器
const root = {
  accounts() {
    // 对象转数组以便输出
    var arr = [];
    for(const key in fakeDb) {
      arr.push(fakeDb[key])
    }
    return arr;
  },
  createAccount({input}) {
    // 相当于数据库的保存
    fakeDb[input.name] = input;
    return fakeDb[input.name];
  },
  updateAccount({id, input}) {
    // 相当于数据库的更新
    const updatedAccount = Object.assign({}, fakeDb[id], input);
    fakeDb[id] = this.updatedAccount;
    return updatedAccount;
  }
}

const app = express();

const middleware = (req, res, next) => {
  // 手动更改cookie，实战中需要后端生成token给前端
  if(req.url.indexOf('/graphql') !== -1 && req.headers.cookie.indexOf('auth') === -1) {
    res.send(JSON.stringify({
      error: "您没有权限访问这个接口"
    }));
    return;
  }
  next();
}

app.use(middleware);

app.use('/graphql', graphqlHTTP({
  schema: schema,
  rootValue: root,
  graphiql: true
}))

app.listen(3000);
```