---
title: svg分图层显示图像
date: 2019-07-30 18:56:06
tags:  
  - JavaScript
  - svg
categories: 前端
---
## 一、问题
给定的数组里包含位置相同但图层（layer，相当于z-index）不同的元素，但是本该位于下层的元素遮住了上层的元素，导致部分元素无法正常显示。

## 二、原因
svg规定：先添加至节点树的元素位于z-index底层，后添加的元素位于上层。然而给定的数组里图层元素排列混乱。
![avatar](https://img2018.cnblogs.com/blog/1549437/201907/1549437-20190730185110102-1647068539.png)

## 三、解决
将数组按图层顺序有小到大排序：
``` js
// iteration function, make array listed by layer from low to high
for(var m = 0; m < arr.length - 1; m++) {
    for (var n = 0; n < arr.length - m - 1; n ++) {
        if (arr[n].layer > arr[n + 1].layer ) {
            swapArr(arr, n, n + 1);
        } 
    }
}

// swap elements in arr
function swapArr(arr, i1, i2) {
    arr[i1] = arr.splice(i2, 1, arr[i1])[0];
    return arr;
}
```

## 四、效果
![avatar](https://img2018.cnblogs.com/blog/1549437/201907/1549437-20190730185458230-290538758.png)

svg终于正常显示...orz。

