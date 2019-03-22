---
title: 重学jQuery API
date: 2019-03-16 22:51:33
tags: [jQuery]
categories:
---

本来看jQuery源码的，发现不是现阶段看的东西，最好还是把jquery api查看一下
<!-- more -->
# api开始看起

# .add()
将匹配元素添加到集合中
```js
$('.j').add('.name').css({'background': 'red'}) //会给匹配到的.j .name都添加背景色
```
# .addBack()
这个就是将前面匹配的元素组合到一起
```js
$('.j').find('p').addBack().addClass('background');//给匹配到的P元素添加class，匹配到的.j class也添加class
```
# .addClass()
顾名思义添加class
可以接收一个字符串，或者一个回调函数，
```js
$('.j').addClass(function (index, currentClass) {
	return 'item-' + index
	//currentClass就是当前元素的Class，这里面的this代表元素本身，chrome只能看到元素，firefox可以看到元素的属性和方法，如果用ES6的箭头函数则this代表window对象
})
```
# .after()
给匹配元素集合中插入参数所指定的内容
```js
$('.inner').after(function () {
  return '<div>' + this.className + '</div>'
});
//可以接收一个函数，或者直接传参，有函数操作起来多样性this和上面一样
```



# .ajaxComplete()
请求完成时注册一个回调函数，和start很像
```js
$(document).ajaxComplete(function() {
  console.log('s')
  $( ".log" ).text( "Triggered ajaxComplete handler." );
});
```

##  .ajaxError()
Ajax请求失败，触发该回调函数

## .ajaxSend()
ajax发送请求前执行

## .ajaxStart()
ajax发送刚开始执行

## .ajaxStop()
ajax发送完成后执行

# append()
和after() 好像啊，一样的？
匹配到的元素后添加
## appendTo
匹配到的元素前

# .attr()
获取匹配元素的属性或设置属性的值
# 选择器
模板
```html
    <div class="hom" wot="cn">
      use name
    </div>
    <div class="hom" wot="cn n">
      use name
    </div>
    <div class="hom" wot="cnnnbbb-n">
      use name
    </div>
    <div class="hom" wot="cn-youcan">
      use name
    </div>
```
## [name|=value]
可以匹配到上面
```js
$('div[wot|=cn]'
//可以匹配到cn-开头的
```

## [name*=value]
```js
//只要是包含cn的，都可以匹配到
```
## [name$=value]
```js
value结尾的
```

## [name~=value]
```js
以空格间隔的
cn n
```

## [name=value]
```
绝对匹配
```

## [name$=value]
```js

```


# tab 开发

## $.extend()
可以使用深度拷贝，但是本节用法好像就是普通合并？
直接=赋值好像也可以？


## $.parseJSON()
```js
接收一个json字符串，返回解析后的javascript对象
```

## .siblings()
匹配元素的其他同级元素

## .trigger() 
手动触发事件

有个事件冒泡bug，是这个解决的
[阻止事件冒泡](https://www.imooc.com/qadetail/213077)

# 防抖处理

## 向量
**向量： Vab = Pb -Pa**
**二维向量叉乘公式：**
a(x1, y1) * b(x2,y2) = x1*y2 - x2*y1

**用叉乘法判断点在三角形内**














学习视频：[逐行分析jQuery](https://study.163.com/course/courseMain.htm?courseId=465001)
jQuery学习版本：2.0.3
## 第一节
匿名函数自执行
```js
(function () {
  var a  = 10;
  function $() {
    alert(a)
  }
  windows.$ = $ //暴露出去
})()
```

## js继承机制
用构造函数生成的实例对象，有一个缺点，那就是无法共享属性和方法

```js
function Dog(name) {
  this.name = name
  this.sg = '犬科'
}

var s = new Dog('大毛')
var s2 = new Dog('二毛')
s2.sg = '猫科'
console.log(s) //犬科
//犬科的方法无法继承

function Dog(name) {
  this.name = name
}
  Dog.prototype = {
    sg = '猫科'
}

var s = new Dog('大毛')
var s2 = new Dog('二毛')
s2.sg = '猫科'
console.log(s) //猫科
//写在原型上的就会被继承
//s2实现了继承
```

## 第四节
匿名函数自执行
```js
(function (window) {

})(window)
```
之所以传入window有两个原因:
1. 可以提快window的查找速度
2. 压缩代码后，有利于代码压缩如下：
```
(function(e){})(window)
```
接收的window会被替换为e

接收undefined
```
(function (window, undefined) {

})(window)
```
接收一个形参，undefined，防止被外部修改
因为undefined非关键字，非保留字，所以低版本浏览器bug可以修改，IE8一下浏览器都有这个bug
一下是修改undefined例子：
```js
var undefined = 15;
(function () {
  var a;
  // 如果a没有值则为a赋值.  但因为undefined被重写了，所以这句根本执行不到。
  console.log(a, undefined);
  if (a === undefined) {
      a = 'b';
  }
  console.log(a);
})();
```
参考文章: [为什么要传入 undefined](https://blog.csdn.net/qjx3936888/article/details/83858467)


## 第五节
jQuery设计原理
```js
function jQuery () {
  return new jQuery.prototype.init()
}
  jQuery.prototype = {
    init: function () {

      },
    css: function () {
      console.log('nice')
    }
  }
jQuery.prototype.init.prototype = jQuery.prototype
jQuery().css()

```

## 第七节
1. 为什么要把constructor重新指向的原因：

```
function Arr() {

}
Arr.prototype = {
  name = 'hhh'
}
var a1 = new Arr()
console.log(a1.constructor) //Object()

```
因为这样写会把prototype赋值一个新的对象
而这样写就不会
```
function Arr () {

}
Arr.prototype.name = 'hhh'
var a1 = new Arr()
console.log(a1.constructor) //Arr()
```
这样写就不会改变constructor
不过对象的方式比较方便，常用所以改下成下面的写法
```
function Arr() {}
Arr.prototype = {
  constructor: Arr,
  name: 'hhh'
}
var a1 = new Arr()
console.log(a1.constructor) //Arr()

```
