---
title: react hooks的基本实现
date: 2022-02-18 16:30:12
tags: 
  - React
  - 基础原理
categories: 前端  
---

一、useState
自定义一个立即执行函数：ReactX
```javascript
const ReactX = (() => {
  let state;

  const useState = (initialValue) => {
    if (state == undefined) {
      state = initialValue;
    }

    const setterFunction = (newValue) => {
      state = newValue;
    }

    return [state, setterFunction];
  };

  return {
    useState,
  }
})();
```
使用useState：
```javascript
const Component = () => {
  const [counterValue, setCounterValue] = useState(1);

  console.log(counterValue);

  if (counterValue !== 2) {
    setCounterValue(2);
  }

};

Component(); // 1
Component(); // 2
```
二、升级useState
真实的state是array的形式：
```javascript
let hooks = [];

let index = 0;
const useState = (initialValue) => {
  const localIndex = index;
  index++;

  if (hooks[localIndex] == undefined) {
    hooks[localIndex] = initialValue;
  }

  const setterFunction = (newValue) => {
    hooks[localIndex] = newValue;
  }

  return [hooks[localIndex], setterFunction];
};
```
此时运行：
```javascript
Component(); // 1
Component(); // 1
```
结果变成：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1645171823696-2507cc95-fdaf-45ba-a837-35ccc8aa544e.png#clientId=u8de6aae3-e6b4-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=90&id=uabbccd0b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=90&originWidth=159&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2472&status=done&style=none&taskId=ucb54a011-a50a-431d-a2f8-6290bd36ab5&title=&width=159)
会发现值没有更新，因此需要一个新的resetIndex函数：
```javascript
const resetIndex = () => {
  index = 0;
}
```
然后再次使用即可得到预期结果：
```javascript
const { useState, useEffect, resetIndex } = ReactX;

const Component = () => {
    const [counterValue, setCounterValue] = useState(1);

    if (counterValue !== 2) {
        setCounterValue(2);
    }

    console.log('counterValue', counterValue);

};

Component(); // 1
resetIndex();
Component(); // 2
```
此时hooks的arr里只有一个值，即为最新值：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1645172110189-e278fc36-0fb2-457a-809e-0180985c78e5.png#clientId=u8de6aae3-e6b4-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=86&id=uac951beb&margin=%5Bobject%20Object%5D&name=image.png&originHeight=86&originWidth=133&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2276&status=done&style=none&taskId=u79b68773-3dee-4227-94b2-e8219e6c785&title=&width=133)
三、useEffect
useEffect需要根据传入的依赖是否改变而判断是否需要更新值：
```javascript
const useEffect = (callback, dependencyArray) => {
  let hasChanged = true;

  const oldDependencies = hooks[index];

  if (oldDependencies) {
    hasChanged = false;

    dependencyArray.forEach((dependency, index) => {
      const oldDependency = oldDependencies[index];
      const areTheSame = Object.is(dependency, oldDependency);
      if (!areTheSame) {
        hasChanged = true;
      }
    });
  }

  if (hasChanged) {
    callback();
  }

  hooks[index] = dependencyArray;
  index++;
}
```
使用useEffect同样需要需要resetIndex，否则useEffect的callback函数会重复被调用：
```javascript
const { useEffect, resetIndex } = ReactX;

const Component = () => {
    useEffect(() => {
        console.log("useEffect");
    }, []);
};

Component();
resetIndex();
Component();
```
结果：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1645172424882-ac866cc9-efa4-426e-b903-1d90b2fec75e.png#clientId=u8de6aae3-e6b4-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=72&id=ua43a2dce&margin=%5Bobject%20Object%5D&name=image.png&originHeight=72&originWidth=207&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1746&status=done&style=none&taskId=ubf0d5f37-360d-41e5-ad2f-d6c08c6a369&title=&width=207)
源码：[https://github.com/bbcfive/react-hooks-demo/blob/main/ReactX.js](https://github.com/bbcfive/react-hooks-demo/blob/main/ReactX.js)
参考视频：[https://www.youtube.com/watch?v=1VVfMVQabx0](https://www.youtube.com/watch?v=1VVfMVQabx0)
