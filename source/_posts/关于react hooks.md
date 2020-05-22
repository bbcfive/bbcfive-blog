---
title: 关于react hooks
date: 2020-01-18 22:56:12
tags: 
  - JavaScript
  - react
categories: 前端  
---
## 一、react的组件
react的核心是组件，react有两种组件类：有状态组件（class）和无状态组件（function）。
有状态组件（class）常常使代码变的冗余而复杂，例如下面一个简单的button组件：
``` js
import React, { Component } from "react";

export default class Button extends Component {
  constructor() {
    super();
    this.state = { buttonText: "Click me, please" };
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    this.setState(() => {
      return { buttonText: "Thanks, been clicked!" };
    });
  }
  render() {
    const { buttonText } = this.state;
    return <button onClick={this.handleClick}>{buttonText}</button>;
  }
}
```
可以看出代码已经很重了。

Redux 的作者 Dan Abramov 总结了组件类的几个缺点。
* 大型组件很难拆分和重构，也很难测试。
* 业务逻辑分散在组件的各个方法之中，导致重复逻辑或关联逻辑。
* 组件类引入了复杂的编程模式，比如 render props 和高阶组件。
因此，组件的最佳实现方式应该是函数，而不是类。

## 二、react hooks——加强版函数组件
Hook 这个单词的意思是"钩子"。
React Hooks 的意思是，组件尽量写成纯函数，如果需要外部功能和副作用，就用钩子把外部代码"钩"进来。 React Hooks 就是那些钩子。
所有的钩子默认使用use前缀命名。
比如以下几个常见的钩子：
* useState()
* useContext()
* useReducer()
* useEffect()
* useCallback()

## 三、状态钩子useState()
<font color="#FF4500">useState()用于为函数组件引入状态（state）。纯函数不能有状态，所以把状态放在钩子里面。</font>
经过状态钩子改造后的button代码：

``` js
import React, { useState } from "react";

export default function  Button()  {
  const  [buttonText, setButtonText] =  useState("Click me,   please");

  function handleClick()  {
    return setButtonText("Thanks, been clicked!");
  }

  return  <button  onClick={handleClick}>{buttonText}</button>;
}
```
再看一个例子，钩前组件代码（有状态）：
``` html
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```
加钩子后的代码（无状态）：
``` html
import { useState } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
useState()这个函数接受状态的初始值，作为参数。该函数返回一个数组，数组的第一个成员是一个变量，指向状态的当前值。第二个成员是一个函数，用来更新状态，约定是set前缀加上状态的变量名。

## 四、共享状态钩子useContext()
现在有两个组件 Navbar 和 Messages，我们希望它们之间共享状态。
``` html
<div className="App">
  <Navbar/>
  <Messages/>
</div>
```
第一步就是使用 React Context API，在组件外部建立一个 Context。并封装它：
``` html
const AppContext = React.createContext({});
<AppContext.Provider value={{
  username: 'superawesome'
}}>
  <div className="App">
    <Navbar />
    <Messages />
  </div>
</AppContext.Provider>
```
上面代码中，AppContext.Provider提供了一个 Context 对象，这个对象可以被子组件共享。
Navbar 组件的代码如下：
``` html
const Navbar = () => {
  const { username } = useContext(AppContext);
  return (
    <div className="navbar">
      <p>AwesomeSite</p>
      <p>{username}</p>
    </div>
  );
}
```
还有许多hooks的示例用法可以参考官网。根据已有的hooks，还可以组合封装，创建出新的hooks来使用。

## 参考文献：
1. [React Hooks 入门教程](http://www.ruanyifeng.com/blog/2019/09/react-hooks.html)
2. [React Hooks](https://www.jianshu.com/p/76901410645a)