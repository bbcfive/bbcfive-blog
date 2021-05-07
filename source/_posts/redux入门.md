---
title: redux入门
date: 2019-01-21 15:02:08
tags: 
  - JavaScript 
  - redux
categories: 前端
---

## 一、redux简介
redux是JavaScript`状态容器`，提供可预测化的状态管理。
它可以运行于不同环境（客户端、服务器和移动端）。

## 二、redux-devtools-extension
redux的时间旅行调试器可以编辑后实时预览。
使用方法：
1. npm redux-devtools-extension
``` js
import { composeWithDevTools } from 'redux-devtools-extension';
composeWithDevTools(applyMiddleware())
```
2. store内加入行代码
``` js
const store = createStore(
  rootReducer,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);
```

## 三、状态管理库的作用
实现日志打印，热加载，时间旅行，同构应用，录制和重放

## 四、不需要用babel打包
Redux 源文件由 ES2015 编写，但是会预编译到 CommonJS 和 UMD 规范的 ES5，所以它可以支持 任何现代浏览器。不必非得使用 Babel 或模块打包器来使用 Redux。

## 五、工作原理
所有的state以一个Json树形式存储在全局唯一的store中，唯一改变state的办法是触发一个action，为了描述action如何改变了state树，需要编写reducers。

``` js
import { createStore } from 'redux';

// 创建reducers
function counter(state = 0, action) {
  switch (action.type) {
  case 'INCREMENT':
    return state + 1;
  case 'DECREMENT':
    return state - 1;
  default:
    return state;
  }
}

// 创建 Redux store 来存放应用的状态。
// API 是 { subscribe, dispatch, getState }。
let store = createStore(counter);

// 可以手动订阅更新，也可以事件绑定到视图层。
store.subscribe(() =>
  console.log(store.getState())
);

// 改变内部 state 惟一方法是 dispatch 一个 action。
// action 可以被序列化，用日记记录和储存下来，后期还可以以回放的方式执行
store.dispatch({ type: 'INCREMENT' });
// 1
store.dispatch({ type: 'INCREMENT' });
// 2
store.dispatch({ type: 'DECREMENT' });
// 1

// 如果有很多个reducers，可以直接bind到props
const mapDispatchToProps = (dispatch) => {
    return bindActionCreators(reducers, dispatch);
};

// 再connect到组件里
export default connect(mapStateToProps, mapDispatchToProps)(App);
```

## 六、中间件
通俗理解就是：以最小的代价来最大限度的复用逻辑。 =》 中间商 =》 中介
前端开发中的中间件：以koa为例，其实就是generator+http 
参考：
1.http://zhenhua-lee.github.io/node/middleware.html
2.https://guoyongfeng.github.io/book/15/02-%E5%88%9D%E6%AD%A5%E7%90%86%E8%A7%A3middleware%E4%B8%AD%E9%97%B4%E4%BB%B6.html

## 七、基础
action：把数据传到store的唯一载荷，通过store.dispatch()将action传到store。store 里能直接通过 store.dispatch() 调用 dispatch() 方法，但是多数情况下你会使用 react-redux 提供的 connect() 帮助器来调用。bindActionCreators() 可以自动把多个 action 创建函数 绑定到 dispatch() 方法上。
reducer：描述如何更新state，使store内的具体数据变换对应相应的action。表达式：(previousState, action) => newState
store：在使用reducer来根据action更新state时，store的作用有：1. 维持应用的 state；2. 提供 getState() 方法获取 state；3. 提供 dispatch(action) 方法更新 state；4. 通过 subscribe(listener) 注册监听器; 5. 通过 unsubscribe(listener) 返回的函数注销监听器。



