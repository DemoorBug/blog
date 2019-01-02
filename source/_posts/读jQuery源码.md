---
title: 读jQuery源码
date: 2019-01-02 20:38:33
tags: [jQuery]
categories:
---

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