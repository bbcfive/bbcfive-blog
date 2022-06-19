---
title: 异步的JavaScript和Event Loop
date: 2022-02-09 18:03:08
tags: 
  - 浏览器Chrome
  - 基础知识
categories: 前端
---

#### 一、JavaScript的执行
JS是一种单线程语言，它只有一个call stack，所以程序块会被JS engine逐行执行。例如：
```javascript
function a() {
	console.log('a');
}
a();
console.log('b');

// console
a
b
```
如果想让JS执行一些异步操作任务，则需要借助浏览器的其他能力。
#### 二、browser背后的操作
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1644203586170-20e1e767-8489-419b-a43b-fd8ec9a6d39b.png#clientId=ubb30a9d2-e099-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=537&id=ucbce3bb4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=537&originWidth=835&originalType=binary&ratio=1&rotation=0&showTitle=false&size=360310&status=done&style=none&taskId=ubae5a9d6-85d7-42d4-9c3c-479a260c0a7&title=&width=835)
browser为JS执行提供了运行时环境和丰富的扩展工具，例如：UI界面，导航栏，timer，localstorage和location...
JS可以直接使用browser提供的功能，以扩充自身的语言能力。
#### 三、Web api
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1644204609576-368f473a-ffde-4d27-8291-8cd937b86418.png#clientId=ubb30a9d2-e099-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=869&id=u9c5fcf5d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=869&originWidth=996&originalType=binary&ratio=1&rotation=0&showTitle=false&size=520290&status=done&style=none&taskId=u6a4b6123-60e6-46ff-b832-02757a1cb48&title=&width=996)
#### 四、setTimeOut是如何工作的
JS在开始执行如下代码段的时候，首先会在call stack里注册gec(global excution context)，
```javascript
console.log('start');
setTimeOut(function cb() {
	console.log('cb func');
}, 5000);
console.log('end');

// result
start
end
cb func
```
然后顺序执行代码块，在遇到setTimeOut时，会将cb函数注册到Web api的环境里，同时在浏览器提供的timer工具中开始计时，当5000ms结束，cb函数会被push到callback queue里，此时event loop会不停地检测call stack，当gec代码块里的内容被执行完后，event loop发现call stack为空，此时会将callback queue里的cb函数弹出并push到call stack然后立即执行。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1644205459757-6dd7d281-18fc-4859-b99d-3cfce93ed8be.png#clientId=ubb30a9d2-e099-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=595&id=ub615f62d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=595&originWidth=907&originalType=binary&ratio=1&rotation=0&showTitle=false&size=437869&status=done&style=none&taskId=ue7cce699-377c-458b-859b-3ace5259326&title=&width=907)
JS语言通过这样一种方式实现了异步操作。
#### 五、fetch是如何工作的
fetch的执行方式与其他web api稍有不同，以此代码段为例：
```javascript
console.log('start');
setTimeOut(function cbT() {
	console.log('cb timer');
}, 5000);
fetch('http://www.baidu.com').then(function cbF() {
	console.log('cb fetch');
}); // 此处假设fetch的结果返回只需50ms
console.log('end');

// result
start
end
cb fetch
cb timer
```
JS会先在call stack里注册gec，然后逐行执行非异步的代码，当遇到setTimeOut时，将其推进callback queue里，当遇到fetch时，则会将其推进micro task queue里。
micro task queue里的任务会比callback queue里的任务优先级高，所以将被优先执行。
当event loop检测到call stack里的gec被执行完后，会优先处理micro task queue里的任务，然后才是callback queue里的任务。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2431928/1644206221377-f15f6ef1-5218-4426-a4b0-2d1d022c466d.png#clientId=ubb30a9d2-e099-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=595&id=ucaf30536&margin=%5Bobject%20Object%5D&name=image.png&originHeight=595&originWidth=928&originalType=binary&ratio=1&rotation=0&showTitle=false&size=531175&status=done&style=none&taskId=u736a2f54-0553-455a-b038-9335b060167&title=&width=928)
#### 六、什么样的任务会被放进micro task queue？
一般是Promise，mutation observer之类的cb会进入micro task queue，其他异步的cb会进入callback queue。
上述异步的cb都会在web api environment里被注册和保存，然后其reference会被push到queue里执行。
剩余同步的cb（例如：map，filter）会直接被执行，而不会在web api environment里注册。
#### 七、Q&A
Q: How does it matter if we delay for setTimeout would be 0ms. Then callback will move to queue without any wait?
A: YES, If delay for setTimeout is 0 ms, a callback will be registered with 0 ms timeout and will immediately be pushed into the callback queue. However, the event loop will only move it to the JS call stack after it becomes empty. Hence, you can use setTimeout with 0ms wait time to defer a callback execution.
#### 八、一个简易的event loop
```javascript
while (true) {
   if (isCallStackEmpty()) {
       task = pickFirstTaskFromMicroTaskQueue() || pickFirstTaskFromCallbackQueue()
       if (task)
            CALLSTACK.push(task)
    }
}
```
#### 九、关于callback(macrotasks)和microtask queue的区别
> One go-around of the event loop will have **exactly one** task being processed from the **macrotask queue** (this queue is simply called the _task queue_ in the [WHATWG specification](https://html.spec.whatwg.org/multipage/webappapis.html#task-queue)). After this macrotask has finished, all available **microtasks** will be processed, namely within the same go-around cycle. While these microtasks are processed, they can queue even more microtasks, which will all be run one by one, until the microtask queue is exhausted.
> ## What are the practical consequences of this?
> If a **microtask** recursively queues other microtasks, it might take a long time until the next macrotask is processed. This means, you could end up with a blocked UI, or some finished I/O idling in your application.
> However, at least concerning Node.js's process.nextTick function (which queues **microtasks**), there is an inbuilt protection against such blocking by means of process.maxTickDepth. This value is set to a default of 1000, cutting down further processing of **microtasks** after this limit is reached which allows the next **macrotask** to be processed)
> ## So when to use what?
> Basically, use **microtasks** when you need to do stuff asynchronously in a synchronous way (i.e. when you would say _perform this (micro-)task in the most immediate future_). Otherwise, stick to **macrotasks**.
> ## Examples
> **macrotasks:** [setTimeout](https://developer.mozilla.org/docs/Web/API/WindowTimers/setTimeout), [setInterval](https://developer.mozilla.org/docs/Web/API/WindowTimers/setInterval), [setImmediate](https://developer.mozilla.org/docs/Web/API/Window/setImmediate), [requestAnimationFrame](https://developer.mozilla.org/docs/Web/API/window/requestAnimationFrame), [I/O](https://developer.mozilla.org/docs/Mozilla/Projects/NSPR/Reference/I_O_Functions), UI rendering
**microtasks:** [process.nextTick](https://nodejs.org/uk/docs/guides/event-loop-timers-and-nexttick/), [Promises](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise), [queueMicrotask](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/queueMicrotask), [MutationObserver](https://developer.mozilla.org/docs/Web/API/MutationObserver)


附：

1. [Asynchronous JavaScript & EVENT LOOP from scratch 🔥 | Namaste JavaScript Ep.15](https://www.youtube.com/watch?v=8zKuNo4ay8E)

