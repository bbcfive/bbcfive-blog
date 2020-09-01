---
title: 使用GraphQLObjectType定义type
date: 2020-06-03 17:54:08
tags: graphQL
categories: 前端
---

## 使用构造函数GraphQLObjectType定义type，便于提高可维护性（快速定位错误）

``` js
const express = require('express');
const graphql = require('graphql');
const graphqlHTTP = require('express-graphql');

var AccountType = new graphql.GraphQLObjectType({
  name: 'Account',
  fields: {
    name: { type: graphql.GraphQLString },
    age: { type: graphql.GraphQLInt },
    sex: { type: graphql.GraphQLString },
    department: { type: graphql.GraphQLString }
  }
});

var queryType = new graphql.GraphQLObjectType({
  name: 'Query',
  fields: {
    account: {
      type: AccountType,
      args: {
        username: { type: graphql.GraphQLString }
      },
      resolve: function (_, { username }) {
        const name = username;
        const sex = 'male';
        const age = 18;
        const department = 'school';
        return {
          name,
          sex,
          age,
          department
        }
      }
    }
  }
});

var schema = new graphql.GraphQLSchema({ query: queryType });

const app = express();

app.use('/graphql', graphqlHTTP({
  schema: schema,
  graphql: true
}));

app.listen(3000);
```