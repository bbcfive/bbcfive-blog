---
title: Typescript 中的 interface 和 type 到底有什么区别
date: 2020-09-01 15:30:57
tags: TypeScript
categories: 前端
---
本文是对[Typescript 中的 interface 和 type 到底有什么区别](https://juejin.im/post/6844903749501059085)的阅读理解总结。

## 一、定义
1. interface : An interface can be named in an extends or implements clause, but a type alias for an object type literal cannot.
2. type : An interface can have multiple merged declarations, but a type alias for an object type literal cannot.

## 二、相同点
1. 都可以描述一个对象或者一个函数
    interface
    ```
    interface User {
      name: string
      age: number
    }

    interface SetUser {
      (name: string, age: number): void;
    }
    ```

    type
    ```
    type User = {
      name: string
      age: number
    };

    type SetUser = (name: string, age: number)=> void;
    ```
  
2. 都允许扩展，其中type和interface可以互相扩展
    interface extends interface
    ```
    interface Name { 
      name: string; 
    }
    interface User extends Name { 
      age: number; 
    }
    ```

    type extends type
    ```
    type Name = { 
      name: string; 
    }
    type User = Name & { age: number  };
    ```

    interface extends type
    ```
    type Name = { 
      name: string; 
    }
    interface User extends Name { 
      age: number; 
    }
    ```

    type extends interface
    ```
    interface Name { 
      name: string; 
    }
    type User = Name & { 
      age: number; 
    }
    ```

## 三、不同点
type 可以而 interface 不行
1. type可以声明基本类型别名，联合类型，元组等类型
   ```
    // 基本类型别名
    type Name = string

    // 联合类型
    interface Dog {
        wong();
    }
    interface Cat {
        miao();
    }

    type Pet = Dog | Cat

    // 具体定义数组每个位置的类型
    type PetList = [Dog, Pet]
    ```
  

2. 其他操作
    ```
    type StringOrNumber = string | number;  
    type Text = string | { text: string };  
    type NameLookup = Dictionary<string, Person>;  
    type Callback<T> = (data: T) => void;  
    type Pair<T> = [T, T];  
    type Coordinates = Pair<number>;  
    type Tree<T> = T | { left: Tree<T>, right: Tree<T> };
    ```

interface 可以而 type 不行
3. interface 能够声明合并(重载)
  ```
  interface User {
    name: string
    age: number
  }

  interface User {
    sex: string
  }

  /*
  User 接口为 {
    name: string
    age: number
    sex: string 
  }
  */
  ```


