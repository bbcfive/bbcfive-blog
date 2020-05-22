---
title: TypeScript + node.js + github.api, 实现github快速找到目标用户
date: 2019-07-05 22:41:01
tags:  
  - TypeScript
  - NodeJs
categories: 前端
---

## 一、TypeScript简介
1. 定义

（1）Typescript = JavaScript + type + ( some other stuff )

（2）Typescript需要先被编译成Javascript，而后才能使用

2. demo
  安装typescript：
    * npm install typescript –g
    * tsc index.ts

index.ts：
> var fn = () => ‘response’

command line：
> tsc init

修改tsconfig.json的outDir和rootDir，并增添out目录与src目录
修改package.json的scripts为"start": "tsc && node out/index.js"
运行npm start

结果如下：

![avatar](https://img2018.cnblogs.com/blog/1549437/201907/1549437-20190705222224250-257052105.png)

此时，src里的index.ts已经被编译至index.js中

index.js：
> var fn = function () { return ‘response’; }

## 二、项目实现
1. 需求
在node环境下，使用ts语言，借用gituhub的api实现对用户的关键信息查询功能

2. 目录

![avatar](https://img2018.cnblogs.com/blog/1549437/201907/1549437-20190705222612450-142938930.png)

3. 安装库
（1）request库
  npm i @types/request --save-dev
  npm i request –save
（2）lodash库（用于排序）
  npm i @types/lodash --save-dev
  npm install lodash --save-dev

4. 内容
index.ts
``` ts
import { GithubApiService } from './GithubApiService'
import { User } from './User'
import { Repo } from './Repo';
import * as ld from 'lodash';

// node.js内置功能，用于获取第三个参数内容
console.log(process.argv[2])

let svc: GithubApiService = new GithubApiService();

if (process.argv.length < 3) {
    console.log("请输入用户名");
} else {
    svc.getUserInfo("bbcfive", (user: User) => {
        svc.getRepos(user.login, (repos: Repo[]) => {
            // 按照fork的数量由小到大排列 ps：返回值 * -1 代表由大到小排列
            let sortedRepos = ld.sortBy(repos, [(repo: Repo) => repo.forks_count]);
            user.repos = sortedRepos;
            console.log(user);
        })    
    }); 
}
```

GithubApiService.ts
``` ts
import * as request from 'request'
import { User } from './User';
import { Repo } from './Repo';

const options = {
    headers: {
        'User-Agent': 'request'
    },
    json: true
}

export class GithubApiService {
  getUserInfo(userName:string, callBack: (user: User) => any) {
    request.get(
      "http://api.github.com/users/" + userName, 
      options, 
      (error: any, response: any, body: any) 
    => {
      // 写法一
      // let user = new User(JSON.parse(body)); // typeof body == object
      // 写法二 
      let user: User = new User(body); // typeof body == string
      callBack(user);
    })
  }

  getRepos(userName:string, callBack: (repos: Repo[]) => any) {
    request.get(
      "http://api.github.com/users/" + userName + "/repos", 
      options, 
      (error: any, response: any, body: any) 
    => {
      let repos: Repo[] = body.map((repo: any) => new Repo(repo));
      callBack(repos);
    })
  }
}
```

Repo.ts
``` ts
export class Repo {
    name: string;
    description: string;
    url: string;
    size: number;
    forks_count: number;

    constructor (repo: any) {
        this.name = repo.name;
        this.description = repo.description;
        this.url = repo.html_url;
        this.size = repo.size;
        this.forks_count = repo.forks_count;
    }
}
```

User.ts
``` ts
import { Repo } from './Repo';

export class User {
    login: string;
    fullName: string;
    repoCount: number;
    followerCount: number;
    repos: Repo[] = [];

    constructor (userResponse: any) {
        this.login = userResponse.login;
        this.fullName = userResponse.name;
        this.repoCount = userResponse.public_repos;
        this.followerCount = userResponse.followers;
    }
}
```

5. 运行
因为在index.ts里已经使用nodeJs自带的api，process.argv对npm输入命令做了判断，所以只需要在命令行输入
> npm start bbcfive // "bbcfive"是我的github用户名

终端打印结果：

![avatar](https://img2018.cnblogs.com/blog/1549437/201907/1549437-20190705223534662-1742728754.png)

可以清晰的看到此用户的github关键信息。

## 三、其他延展功能

给定一定量的github用户名列表，可以通过此方法循环获取每个用户的关键信息，并作筛选，来获得目标范围用户。

适用于以下场景：

1. hr招人

2. 找一些很赞的项目

3. 社交（毕竟是全球最大的同性交友网站~~）