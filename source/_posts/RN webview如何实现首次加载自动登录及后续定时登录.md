---
title: RN webview如何实现首次加载自动登录及后续定时登录
date: 2019-05-19 14:37:10
tags: 
  - JavaScript 
  - React Native
  - webview
categories: 移动端
---

## 一、首次加载自动登录

1. 问题分析

Rn webview有自动登录机制，即如果是需要登录的页面，在首次登录后，二次进入不需登录。但第一次进入页面必须要进行密码输入和登录操作。所以，如果要实现首次自动登录，必须要在webview加载之前进行手动登录操作。

2. 解决

利用RN webview的<font color="#FF4500">handleWebViewNavigationStateChange</font>和<font color="#FF4500">injectedJavaScript</font>等api进行操作：

webview：
``` html
<WebView
  ref={ref => (this.webview = ref)}
  onNavigationStateChange={this.handleWebViewNavigationStateChange}
  source={{ uri: this.state.source }}
/>
```

handleWebViewNavigationStateChange：
``` js
// 注意，此函数可能会在webview加载时被多次执行，为避免重复fetch请求，需设置一个a值做bool判断
let a = false; 

handleWebViewNavigationStateChange = newNavState => {
  const { url } = newNavState;

  // redirect somewhere else
  // 做关键词判断
  if (url.includes(xxxx) && !a) {   
    fetch( URL , {
        credentials: 'same-origin',
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(BODY)
      }).catch(
        err => {
          throw err;
        }
      ).then(
        resp => {
          let newURL = "xxxxx" // 登录后的跳转URL
          let redirectTo = 'window.location = "' + newURL + '"';　　　　　　　// 注入新的跳转链接
          this.webview.injectJavaScript(redirectTo);         
        }
      );  
      a = true;
  }
};
```
webview通过injectJavaScript注入代码到pc端，进行手动跳转操作，即可达到自动登录效果。

## 二、webview定时自动登录

HTML设定距离最后一次操作的8小时内若无任何用户操作，即会注销用户，提示超时信息。

所以设定每7小时自动登录一次，即可避免页面超时。

handleWebViewNavigationStateChange：
``` js
handleWebViewNavigationStateChange = newNavState => {
  console.log(newNavState)
  const { url } = newNavState;
  var _this = this;
  // redirect somewhere else
  if (url.includes('dom') && !a) {
    //首次登录弹框：首次加载,时间较长,请耐心等待
    this.setState({ firstLogin: true })

    //首次加载自动登录
    this.autoLogin(_this);

    // 每7个小时后定时自动登录
    setInterval(() => {
      this.autoLogin(_this)
    }, 1000*60*60*7)
    a = true;
  }
};
```

autoLogin：
``` js
autoLogin(_this) {
  fetch('http://dom.inwhile.com/api/login/2', {
    credentials: 'same-origin',
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({"name": 'dev@basdom.com', "pwd": "Abcd12345"})
  }).catch(
    err => {
      throw err;
    }
  ).then(
    resp => {
      let newURL = 'http://dom.inwhile.com/app/observer/'
        + this.state.itemId + '/'
        + this.state.name_en + '/'
        + this.state.newId + '/'
        + this.state.page_Id;            
      if( this.state.page_Id !== '' ) {
        this.setState({ source: newURL, firstLogin: false });
        let redirectTo = 'window.location = "' + newURL + '"';
        this.webview.injectJavaScript(redirectTo);                          
      }                 
    }
  );     
}
```

补充：

原本的解决方法（太过鸡肋。。。）如下：

webview过期后自动登录：

当webview加载的网页过期后，HTML端可能会弹出（alert）警告窗口，并跳转到登录页。因此，过期后自动登录的实现需要解决三个问题：获取网页端过期的时间节点提醒、再次自动登录和取消alert提醒（屏蔽弹出框）。

1. 获取HTML过期提醒

HTML需向RN传递消息：
``` js
window.postMessage(JSON.stringify({
    msg: "out of date!"
}))
```

RN接收消息：
``` js
onMessage = event => {

    const action = JSON.parse(event.nativeEvent.data)

    if (action.msg == "out of date") {

    //2.屏蔽alert弹框 
    const spamAlertScript =
    ` 
    alert = function(str) {
        return; //什么事也不做，等于屏蔽了它
    }
    alert( "已经无效！ ");
    ` 
    this.webview.injectJavaScript(spamAlertScript);

    //3.再次自动登录
    setTimeout(() => {
            this.autoLogin(); 
        }, 500);
    }
  }
}
```


