---
title: jQuery知识点学习整理
date: 2018-12-14 16:59:00
tags: 
  - JavaScript
  - jQuery
categories: 前端
---

## 零、jQuery中操作css的方法
1.$("p").css("background-color");
返回首个匹配元素的background-color的值。

2.$("p").css("background-color","red");
设置匹配元素的background-color的值。

3.$("p").css( { "background-color" : "red" } );
作用同2。

question：js 动态改变background:url($cover)值？

answer：把变量作为一个字符串的值即可：
$(".bg_pic").css("background", "url(' " + $cover + " ')" );

## 一、jQuery和JS入口函数的区别
1.原生js和jQuery入口函数加载模式不同
原生js会等到DOM元素加载完毕，并且图片也加载完毕才会执行
jQuery会等到DOM元素加载完毕，但不会等到图片也加载完毕就会执行
2.多个入口函数的覆盖问题
原生js如果编写了多个入口函数，后面编写的会覆盖前面编写的
jQuery中编写了多个入口函数，后面的不会覆盖前面的

## 二、jQuery入口函数其他写法
1.第一种写法：
$(document).ready(function () {
//do sth
});
2.第二种写法：
jQuery(document).ready(function () {
//do sth
});
3.第三种写法：(推荐)
$(function () {
//do sth
});
4.第四种写法：
jQuery(function () {
//do sth
});

## 三、jQuery的"$"符号冲突问题解决方法
1.释放$的使用权
jQuery.noConflict();
attention:
释放操作必须在编写其它jQuery代码之前编写
释放之后就不能再使用$,改为使用jQuery
2.自定义一个访问符号
var jq = jQuery.noConflict();
jq(function () {
//do sth
});

## 四、jQuery的核心函数
$();就代表调用jQuery的核心函数
1.接收一个函数 $(function () {});
2.接收一个字符串
2.1 接收一个字符串选择器 var $box1 = $(".box1");
2.2 接收一个字符串代码片段 var $p = $("<p>我是段落</p>"); $box1.append($p);
2.3 接收一个DOM元素 var span = document.getElementByTagName("span")[0]; var $span = $(span);
(会被包装成一个jQuery对象=>是个伪数组)

## 五、jQuery静态方法与实例方法
原生js的静态方法与实例方法：
静态方法：直接添加到类上的方法；通过类名调用；
实例方法：直接添加到原型上的方法；通过创建类的对象调用；
1.静态方法——each
原生： arr.forEach(function (value, index) {}) //只能遍历数组，不能遍历对象/伪数组
jQuery：$.each(arr, function(index, value) {}) //先遍历index，后遍历值；可遍历对象/伪数组
2.静态方法——map
原生： arr.map(function (index, value, array) {}); //只能遍历数组，不能遍历对象/伪数组
jQuery：$.map(arr, function (value, index) {}); //先遍历index，后遍历值；可遍历对象/伪数组
3.map和each的区别
each默认返回值是所遍历的对象;不支持在回调函数中对所遍历的数组进行处理
map默认的返回值是空数组;支持在回调函数中通过return对所遍历的数组进行处理，并返回一个新数组
3.静态方法——trim
$.trim(str); //去字符串两端的空格
4.静态方法——isWindow
$.isWindow(); //判断是不是window对象
5.静态方法——isArray
$.isArray(arr); //判断是不是Array
6.静态方法——isFunction
$.isFunction(fn); //判断是不是函数
注意：jQuery框架本质上是一个匿名函数
(function (window, undefined) {
//jQuery框架内容
})( window );
7.静态方法——holdReady
$.holdReady(true); //暂停ready事件执行
$.holdReady(false); //恢复ready事件执行

## 六、jQuery的内容过滤选择器
1.:empty
$("div:empty"); //找到既没有文本内容也没有子元素的指定元素
2.:parent
$("div:parent"); //找到有文本内容或有子元素的指定元素
3.:contains(text)
$("div:contains('我是div')"); //找到包含指定文本内容的指定元素
4.:has(selector)
$("div:has('span')"); ////找到包含指定子元素的指定元素

## 七、属性与属性节点
1.什么是属性？ 
对象身上保存的变量
2.如何操作属性？
对象.属性名称 = 值；
对象["属性名称"] = 值；
3.什么是属性节点？
<span name = "sp"></span>
在HTML标签中添加的属性就是属性节点。
在浏览器中找到span这个DOM元素之后，展开看到的都是属性，在attributes属性中保持的所有内容都是属性节点。
4.如何操作属性节点？
DOM元素.setAttribute("属性名称", "值");
DOM元素.getAttribute("属性名称");
5.属性和属性节点有什么区别？
任何对象都有属性，但是只有DOM对象才有属性节点。

## 八、jQuery的attr方法
1.attr(name|pro|key,val|fn)
$("span").attr("class","box");
作用：获取或者设置属性节点的值
传递一个参数：代表获取属性节点的值
传递两个参数：代表设置属性节点的值
attention：
如果是获取：无论找到多少个元素，都只会返回第一个元素指定的属性节点的值
如果是设置：找到多少个元素就会设置多少个元素；如果设置的属性节点不存在，那么系统会自动新增
2.removeAttr(name)
作用：删除属性节点
attention：会删除所有找到元素指定的属性节点
$("span").removeAttr("class name");

## 九、jQuery的prop方法
1.prop方法，特点和attr方法一致
2.removeProp方法，特点和removeAttr方法一致
$("span").eq(0).prop("demo", "pr0");  //eq(0) =》给队列中第0个元素添加
$("span").eq(1).prop("demo", "pr1");
$("span").removeProp("demo");
attention：
1.prop方法不仅能够操作属性，还能够操作属性节点
2.官方推荐在操作属性节点时，具有true和false两个属性的属性节点，如checked、selected或者disabled使用prop()，其他的使用attr()；

## 十、jQuery操作类相关的方法
1.addClass(class|fn)
$("div").addClass("class1 class2");
作用：添加一个类。如果需要添加多个，之间用空格隔开即可
2.removeClass([class|fn])
作用：删除一个类。如果需要删除多个，之间用空格隔开即可
3.toggleClass(class|fn)
作用：切换类。有就删除，没有就添加

## 十一、jQuery文本值相关的方法
1.html([val|fn])      $("div").html();
和原生JS中的innerHTML一模一样
2.text([val|fn])
和原生JS中的innerText一模一样
3.val([val|fn|arr])
原生中的value
attention：innerHTML和innerText的区别：前者会被创建为一个子元素，后者不会

## 十二、jQuery位置和尺寸操作的方法
1.offset()
作用：获取元素距离窗口的偏移位
2.position()
作用：获取元素距离定位元素的偏移位

## 十三、jQuery的scrollTop方法
1.获取滚动的偏移位  (页面右侧的上下滚动条相距窗口顶侧的位置)
var need = $(".scroll").scrollTop();
2.设置滚动的偏移位
$(".scroll").scrollTop(300);

## 十四、jQuery的事件绑定
两种方式：
1.eventName(fn);      $("button").click(function () {})
编码效率略高/ 部分事件jQuery没有实现，所以不能添加
2.on(eventName, fn);      $("button").on("click", function () {})
编码效率略低/ 所有的js事件都可以添加
attention：可以添加多个相同或不同的事件，不会覆盖

## 十五、jQuery的事件移除
1.off方法如果不传递参数，会移除所有的事件
$("button").off();
2.off方法如果传递一个参数，会移除所有指定类型的事件
$("button").off("click");
3.off方法如果传递两个参数，会移除所有指定类型的指定事件
$("button").off("click", test1);

## 十六、jQuery事件冒泡和默认行为
1.什么是事件冒泡
子级事件会向父元素冒泡从而牵动父级事件
2.如何阻止事件冒泡
return false;
event.stopPropagation();
3.什么是默认行为
表单、链接等点击后自动跳转行为
4.如何阻止默认行为
return false;
event.preventDefault();

## 十七、jQuery事件的自动触发
1.$(".class").trigger("click"); $("input[type = 'submit']").trigger("click");
trigger：如果用trigger自动触发事件，会触发事件冒泡；会触发默认行为
2.$(".class").triggerHandeler("click"); $("input[type = 'submit']").triggerHandeler("click");
triggerHandler：如果用triggerHandler自动触发事件，不会触发事件冒泡；不会触发默认行为

## 十八、jQuery自定义事件
$(".class").on("defineClick", function () {});
$(".class").triggerHandler("defineClick");
自定义事件必须满足两个条件：
1.事件必须通过on绑定
2.事件必须通过trigger来触发

## 十九、jQuery事件命名空间
$(".class").on("defineClick.zsj", function () {});
$(".class").trigger("defineClick.zsj");
想要事件命名空间有效，必须满足两个条件：
1.事件必须通过on绑定
2.事件必须通过trigger来触发
attention：
1.利用trigger触发子元素带命名空间的事件，那么父元素带相同命名空间的事件也会被触发，而父元素没有命名空间的事件不会被触发；
2.利用trigger触发子元素不带命名空间的事件，那么子元素所有相同类型的事件和父元素所有相同类型的事件都会被触发。

## 二十、jQuery事件委托
$("ul>li").click(function () {
console.log($(this).html());
});
此时，通过js动态新增li元素时，控制台不会显示新增元素。因为触发的是子级，事件发生时，jQuery会遍历所有已有的元素，给所有找到的元素添加事件。
$("ul").delegate("li","click", function () {
console.log($(this).html());
});
此时，通过js动态新增li元素时，控制台会显示新增元素。因为触发的是父级，将子级的事件委托给父子代理监听，因此任何变化父级都能监测到。

## 二十一、jQuery移入移出事件
1.mouseover/mouseout事件，子元素被移入移出也会触发父元素的事件
2.mouseenter/mouseleave事件，子元素被移入移出不会触发父元素的事件
3.mouseenter + mouseleave事件 = $(元素).hover(function () {
　　//移入做某事
}, function () {
　　//移出做某事
});

## 二十二、jQuery显示和隐藏动画
1.显示动画
$("div").show( 1000, function () {
　　//作用：动画执行完毕后再调用
});
2.隐藏动画
$("div").hide( 1000, function () { } );
3.切换动画
$("div").toggle( 1000, function () { } );

## 二十三、展开和收起动画
1.展开动画
$("div").slideDown( 1000, function () {
　　//作用：动画执行完毕后再调用
});
2.收起动画
$("div").slideUp( 1000, function () { } );
3.切换动画
$("div").slideToggle( 1000, function () { } );
4.停止动画
$("div").stop();

## 二十四、淡入淡出动画
1.淡入动画
$("div").fadeIn( 1000, function () { } );
2.淡出动画
$("div").fadeOut( 1000, function () { } );
3.切换
$("div").fadeToggle( 1000, function () { } );
4.淡入到(什么程度) =>第二个参数是opacity
$("div").fadeTo( 1000, 0.5, function () { } );

关于动画：
1.所有动画前都加$.stop();函数，效果最好
2.有时候动画函数的嵌套效果更合理，比如淡入淡出，不是一行淡入一行淡出，而是在淡入函数的第二个参数里加上淡出函数，这是嵌套关系。
但有时，这种嵌套过多，会影响代码的阅读效率，因此应该如下书写：
$("div").stop().slideDown(1000).fadeOut(1000).fadeIn(1000);   => 有种先后关系

## 二十五、自定义动画
1.$("div").animate( {}, 1000, "linear",function() {});
第一个参数可以加元素的css样式，元素会随动画而改变；第三个参数是速度变化效果，有“匀速”和“变速”两种，默认“swing”。
2.第一个参数的关键字
三种属性：操作属性（width:500）、累加属性（width:"+=100"）、字符串属性（width: "hide/toggle"）

## 二十六、stop()和delay()方法
1.$("div").animate( {}, 1000).delay(2000).animate({}, 1000);
delay的作用是用于高速系统延迟时长
2.$("div").stop();   $("div").stop(false);  $("div").stop(false,false);
立即停止当前动画，继续执行后续动画
3.$("div").stop(true);  $("div").stop(true,false);
立即停止当前和后续所有动画
4.$("div").stop(false,true)
立即完成当前的，继续执行后续动画
5.$("div").stop(true,true)
立即完成当前的，并且停止后续所有的

## 二十七、jQuery添加节点相关方法
1.内部插入
1.1将元素添加到指定元素内部的最后：
append(content|fn)
appendTo(content)
1.2将元素添加到指定元素内部的最前面：
pretend(content|fn)
pretendTo(content)
2.外部插入
2.1将元素添加到指定元素外部的最后：
after(content|fn)
insertAfter(content)
2.2将元素添加到指定元素外部的最前面：
before(content|fn)
insertBefore(content)

## 二十八、jQuery删除节点的相关方法
1.删除指定元素
remove()
2.删除指定元素的内容和子元素，指定元素自身不会被删除
empty()
3.删除指定元素的内容和子元素，保留所有绑定的事件、附加的数据
detach()

## 二十九、jQuery替换节点的相关方法
replaceWith()/replaceAll()
替换所有匹配元素为指定元素
var $h6 = $("<h6>我是标题6</h6>");
->$("h1").replaceWith($h6);
->$h6.replaceAll("h1");

## 三十、jQuery复制节点的相关方法
clone([Even[, deepEven]])
如果传false就是浅复制，传true就是深复制
浅复制只会复制元素，不会复制元素事件
深复制会复制元素和元素事件
var $deep = $("div").clone(true);  //深复制一个元素并添加到页面
$("body").append($deep);