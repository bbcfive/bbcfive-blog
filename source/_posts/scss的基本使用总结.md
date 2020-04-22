---
title: scss的基本使用总结
date: 2020-04-21 10:20:00
tags: 
  - css 
  - scss
categories: 前端
---
前言：本文是对[《SCSS 日常用法》](https://blog.csdn.net/KimBing/article/details/89738636) 一文的提炼总结;

## 一、关系

> 简单来说： scss(less/sass) =  有 逻辑 的 动态 css

1. `less`和`sass`是两种`css`的预编译语言，两种语言是动态的`css`，且最终都会转化成`css`执行。
2. `scss`是`sass3`引入新引入的,其语法完全兼容 `css3`,并且继承了`sass`的强大功能。
3. `scss`和`sass`的区别在于前者语法格式上需要`{}`和`;`,而后者多用缩进表达，语法较为简洁。
![avatat](https://img-blog.csdnimg.cn/2020041117195020.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0tpbUJpbmc=,size_16,color_FFFFFF,t_70)

## 二、引入
一般scss搭配vue使用，即在 `xxx.vue` 中声明引入：
```html
  <style lang='scss'></style>
```
## 三、动态

1. 引入变量
  使用`scss`
``` scss
  // 定义变量 Colors
  $red:       #CD594B !global;
  $yellow:    #F8CE5E;
  $green:     #4B9E65;

  // 定义变量 Unites
  $btn-lineheight: 26px;

  // 使用变量 Colors, Unites
  .btn-success {
    background-color: $green;
    line-height: $btn-lineheight;
    color: #fff;
  }
```
  生成`css`
```css
  .btn-success {
      background-color: #4B9E65;
      line-height: 26px;
      color: #fff;
  }
```
2. 嵌入字符串： `#{ 变量 }`
  使用`scss`
``` scss
  $bg-path: "./img";

  .card{
      background-color: url("#{$bg-path}/card-bg.png" center center);
  }
```
  生成`css`
```css
  .card{
      background-color: url("./img/card-bg.png" center center);
  }
```
3. 导入文件： `@import`
  使用`scss` 
``` scss
  // _variables.scss
  $bg-btn: #ddd;
  $color-btn: #444;
  
  // btn.scss
  @import "_variables";

  .btn{
      display: inline-block;
      padding: 3px 6px;
      background-color: $bg-btn; 
      color: $color-btn;
  }
```
  生成`css`
```css
  .btn{
      display: inline-block;
      padding: 3px 6px;
      background-color: #ddd; 
      color: #444;
  }
```

4. 相互嵌入，扩展： `@mixin` `@include`

  使用`scss` 
``` scss
  // @mixin 如果没有调用，不会被渲染

  @mixin rounded($conor: 5px){ // 定义 mixin 并设置默认值 5px
    -webkit-border-radius: $conor;
    -moz-border-radius: $conor;
    border-radius: $conor;
  }

  .btn-rounded{
    @include rounded(); // 这里引用上面的 mixin，默认值 5px
  }

  .btn-big-rounded{
    @include rounded(10px); // 这里引用上面的 mixin，并设置值 10px
  }
```
  生成`css`
```css
  .btn-rounded{
      -webkit-border-radius: 5px;
      -moz-border-radius: 5px;
      border-radius: 5px;
  }
  .btn-big-rounded{
      -webkit-border-radius: 10px;
      -moz-border-radius: 10px;
      border-radius: 10px;
  }
```

5. 继承： `@extend`
  使用`scss` 
``` scss
  .danger{
    background-color: #FF3B30;
  }
  .round{
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    border-radius: 5px;
  }
  .btn{
    display: inline-block;
    padding: 3px 6px;
  }

  .btn-danger{
    @extend .danger, .round, .btn;
  }
```
  生成`css`
```css
  .danger, .btn-danger {
    background-color: #FF3B30;
  }
  .round, .btn-danger {
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    border-radius: 5px;
  }
  .btn, .btn-danger {
    display: inline-block;
    padding: 3px 6px;
  }
```
## 四、逻辑 
1. 判断： `@if` `@else`
  使用`scss`
``` scss
  @if $name == normal {   // 判断 $name 是否为 'normal'
    color: #333;          // 如果是，内部文字颜色为 #333
    border: 1px solid darken($color, 20%);
  } @else {
    color: white;
    border: 1px solid darken($color, 5%);
  }
```
2. 遍历： `@each`
  使用`scss`
``` scss
  $btn-styles: "normal", "primary", "danger";

  @each $type in $btn-styles {
    .btn-#{$type}{
      background-color: white;
    }
  }
```
  生成`css`
```css
  .btn-normal  { background-color: white; }
  .btn-primary { background-color: white; }
  .btn-danger  { background-color: white; }
```
3. 遍历： `@for`
  使用`scss`
``` scss
  @for $gap from 1 through 8 {
    .pb-#{$gap} {
      padding-bottom: 10px * $gap;
    }
  }
```
  生成`css`
```css
  .pb-1 { padding-bottom: 10px; }
  .pb-2 { padding-bottom: 20px; }
  .pb-3 { padding-bottom: 30px; }
  .pb-4 { padding-bottom: 40px; }
  .pb-5 { padding-bottom: 50px; }
  .pb-6 { padding-bottom: 60px; }
  .pb-7 { padding-bottom: 70px; }
  .pb-8 { padding-bottom: 80px; }
```
4. 函数： `@function` `@return`
  使用`scss`
``` scss
  $root: 14;

  // 定义一个方法用于换算尺寸的方法
  // #{} 是输出字符串的，上面有讲
  @function size($size) {
    @return #{$size/$root}rem
  }

  .font-size-normal {
    font-size: size(16); 
  }
```
  生成`css`
```css
  .font-size-normal {
    font-size: 1.1428571429rem;
  }
```
5. 关系： `&`
  使用`scss`
``` scss
  .btn{
  // btn 初始样式
      &:hover{
          // hover 样式
      }
      &:active{
          // active 样式
      }
  }
```
 生成`css`
```css
  .btn {
  /* btn 初始样式 */
  }
  .btn:hover{
  /* :hover 样式 */
  }
  .btn:active{
  /* :active 样式 */
  }
```

## 五、总结
> 总的来说： `scss` 就是有 `逻辑` 的 `动态` `css` 。