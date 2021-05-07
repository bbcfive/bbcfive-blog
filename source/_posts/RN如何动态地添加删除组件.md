---
title: RN如何动态地添加删除组件
date: 2019-05-19 14:47:10
tags: 
  - JavaScript 
  - React Native
categories: 移动端
---
## 一、需求 

点击时动态地创建或删除组件

## 二、问题

RN是通过state更新页面，没有dom那一套直接添加节点的api

## 三、解决

通过数组的增删改查对动态地改变state，以达到创建或删除组件的效果

添加image：

``` html
//add imgItem
imgArr.push(
  <ImgItem 
    key={ imgNum } 
    componentId={ imgNum } 
    time={ now } 
    avatarSource={ source } 
    getDeleteId={ this.getDeleteId }
  />
);
imgNum++;

this.setState({
  imgList: imgArr 
});
```

删除image：
``` js
getDeleteId = deleteId => {
  // delete imgItem
  var itemIndex = imgArr.filter((item, index) => {
    if(item.props.componentId == deleteId) {
      return index;
    } 
  });
  imgArr = imgArr.slice(0, itemIndex).concat(imgArr.slice(itemIndex + 1));
  
  this.setState({ imgList: imgArr });
}  
```

展示image：
``` html
<View style={{ flex: 8.5 }}>
  {this.state.imgList}
</View>
```