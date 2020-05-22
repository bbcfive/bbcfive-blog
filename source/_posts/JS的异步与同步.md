---
title: JS的异步与同步
date: 2019-04-20 23:51:08
tags: 
  - JavaScript
  - 基础原理
categories: 前端
---

## 一、概念

同步（synchronous）：指在js的主线程上，所有任务被依次执行；

异步（asynchronous）：指任务不进入主线程，进入任务队列（task）；当“任务队列”通知主线程，异步任务才进入主线程执行。

## 二、异步的机制

同步任务都在主线程上执行，形成一个“任务栈”；
异步任务在“任务队列”中放置一个事件；
主线程的“任务栈”执行完毕后，“任务队列”里的任务被唤醒，从而进入主线程执行；
主线程不断重复上面三步。
任务队列里的事件主要指IO设备或用户行为触发的事件。

## 三、异步函数
* setTimeout
* setInterval
* Promise
  三个状态：Pending，Fulfilled，Rejected。
  核心方法：resolve/then/reject/catch/all/race
* async await

四、promise和async/await的区别

Promise

``` js
const makeRequest = () =>
  getJSON()
    .then(data => {
      console.log(data)
      return "done"
    })

makeRequest()
```

Async/Await

``` js
const makeRequest = async () => {
  console.log(await getJSON())
  return "done"
}

makeRequest()
```

综上，Async更简洁。