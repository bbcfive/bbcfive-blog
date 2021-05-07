---
title: 从客户端访问graphQL
date: 2020-06-03 17:18:08
tags: graphQL
categories: 前端
---

## 一、新建入口页面
在主目录下建立public/index.html文件
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <button onclick="getData()">获取数据</button>
</body>
<script>
  function getData() {
    const query = `
      query Account($username: String, $city: String) {
        account(username: $username) {
          name
          age
          sex
          salary(city: $city)
        }
      }
    `

    const variables = {username: 'four', city: 'chongqing'}

    fetch('/graphql', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
      },
      // JSON.stringify <=> JSON.parse
      body: JSON.stringify({
        query,
        variables: variables,
      })
    })
    .then(res => res.json())
    .then(data => console.log(data));
  }
</script>
</html>
```

## 二、配置静态文件
``` js
app.use(express.static('public'))
```

## 三、graphql.js
``` js
const express = require('express');
const {buildSchema} = require('graphql');
const graphqlHTTP = require('express-graphql');

const schema = buildSchema(`
  type Account {
    name: String
    age: Int
    sex: String
    department: String
    salary(city: String): Int
  }
  type Query {
    getClassMates(classNo: Int!): [String]
    account(username: String): Account
  }
`);

const root = {
  getClassMates({ classNo }) {
    const obj = {
      31: ['h', 'l', 'c'],
      61: ['m', 'n', 'j']
    }
    return obj[classNo];
  },
  account({ username }) {
    const name = username;
    const sex = 'female';
    const age = 18;
    const department = 'school';
    const salary = ({city}) => {
      if(city == "beijing" || city == "shanghai" || city == "guangzhou" || city == "shenzhen") {
        return 10000;
      } 
      return 3000;
    }
    return {
      name,
      sex,
      age,
      department,
      salary
    }
  }
}

const app = express();

app.use('/graphql', graphqlHTTP({
  schema: schema,
  rootValue: root,
  graphiql: true
}))

app.use(express.static('public'))

app.listen(3000);
```

## 四、使用
node启动本文件后，在本地页面内的网络请求中可以看到相关数据。