---
title: ES6学习记录
date: 2018-11-30 18:19:17
tags: [javascript]
categories: 笔记MAX
---

ECMAScript 6 入门
-----------------------------
[ECMAScript 6 入门](http://es6.ruanyifeng.com/)
ES6 既是一个历史名词，也是一个泛指，含义是 5.1 版以后的 JavaScript 的下一代标准，涵盖了 ES2015、ES2016、ES2017 等等，而 ES2015 则是正式名称，特指该年发布的正式版本的语言标准。本书中提到 ES6 的地方，一般是指 ES2015 标准，但有时也是泛指“下一代 JavaScript 语言”。

let和const命令
-----------------------------
- ES6 新增了let命令，用来声明变量。它的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。
- const声明一个只读的常量。一旦声明，常量的值就不能改变。
- ES5 只有两种声明变量的方法：var命令和function命令。ES6 除了添加let和const命令，后面章节还会提到，另外两种声明变量的方法：import命令和class命令。所以，ES6 一共有 6 种声明变量的方法。
- "暂时性死区" 这个就是说不能在let之前写变量，必须声明不然报错
  - 只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。
- let不允许在相同作用域内，重复声明同一个变量。
- if (父作用域) {子作用域} 不过现在代码无法实现罢了，
  - for (let i = 0; i < 3; i++) {  let i = 'abc';  console.log(i); }
  - 这串代码输出正确就是3个abc，然后现在的转换工具都是输出一个abc,唯有chrome控制台输出3个abc
- 不存在变量提升，也就是上面说的会出现暂时性死区

块级作用域
let实际上为 JavaScript 新增了块级作用域。
立即执行函数表达式（IIFE）不再必要了
```
// IIFE 写法
(function () {
  var tmp = ...;
  ...
}());

// 块级作用域写法
{
  let tmp = ...;
  ...
}
```
块级作用域与函数声明

ES5规定函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明，ES6明确表示可以在块级作用域中声明，函数声明语句的行为类似于`let`，在块级作用域之外不可引用

考虑到环境导致的行为差异太大，应该避免在块级作用域内声明函数，如果确实需要，也应该写成函数表达式，而不是函数声明语句

```
// 函数声明语句
{
  let a = 'secret';
  function f() {
    return a;
  }
}

// 函数表达式
{
  let a = 'secret';
  let f = function () {
    return a;
  };
}
```

#### const 一旦声明必须赋值，不然就会报错

#### 本质
const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，const只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了。因此，将一个对象声明为常量必须非常小心。

#### 对象冻结
const foo = Object.freeze({}); 不知道什么意思


顶层对象的属性与全局变量挂钩，被认为是 JavaScript 语言最大的设计败笔之一。

#### 为什么一段代码要同时运行在浏览器端和node端，为什么同时要区顶层对象，不懂了


变量的解构赋值
-----------------------------
- ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

- npm cache clean --force 搭建环境的时候npm报错，这个命令解决了问题

#### 对象赋值，正真赋值的是后者而不是前者
```
let { foo } = { foo: 'aaa' }
换一种写法
let { foo: baz } = { foo: 'aaaa' }
```
上面的代码中，`foo`是匹配模式，`baz`才是变量，正真被赋值的是变量`baz`,而不是模式`foo`
如果foo也想取到值则要这样写
```
let { foo, foo: baz } = { foo: 'aaaa' }
```
默认值生效的规则是必须严格等于undefined
```
var [x = 3] = [];

var {x = 3} = {x: undefined}

var {x: y = 3} = {}
```

以上是数组和对象的默认值写法

```
// 错误的写法
let x;
{x} = {x: 1};
// SyntaxError: syntax error

```
不能这样写{x}javascript引擎会将它理解为一个代码块，从而发生语法错误

```
// 正确的写法
let x;
({x} = {x: 1});
```

#### 可以这样用，好方便啊
```
let { log, sin, cos } = Math;
```

由于数组本质是特殊的对象，因此可以对数组进行对象属性的解构。
```
let arr = [1, 2, 3];
let {0 : first, [arr.length - 1] : last} = arr;
first // 1
last // 3
```
[arr.length - 1]属于“属性名表达式”（参见《对象的扩展》一章）

#### 字符串的解构赋值
我奇怪的是对象可以用一个方法去解构赋值
```
let {length: len} = 'hello';
len //5
```
那就是说我可以用对象里面的属性喽？


解构赋值时，如果等号右边是数组和布尔值，则会先转为对象。
```
let {toString: a} = 123;
a === Number.prototype.toString   //true

let {toString: b} = true;
b === Number.prototype.toString   //true

```

如果是布尔值或者数字

主要用途也是很广泛的，列举一下
1. 交换变量的值
2. 从函数返回多个值
3. 函数参数的定义
4. 提取 JSON 数据
5. 函数参数的默认值
6. 遍历 Map 结构
7. 输入模块的指定方法

字符串扩展
-----------------------------
- ES6 提供了String.fromCodePoint方法，可以识别大于0xFFFF的字符，弥补了String.fromCharCode方法的不足。在作用上，正好与codePointAt方法相反。
- ES6 提供字符串实例的normalize()方法，用来将字符的不同表示方法统一为同样的形式，这称为 Unicode 正规化
- 传统上，JavaScript 只有indexOf方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6 又提供了三种新方法。
  - includes()：返回布尔值，表示是否找到了参数字符串。
  - startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部
  - endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。
- repeat方法返回一个新字符串，表示将原字符串重复n次。
- ES2017 引入了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。padStart()用于头部补全，padEnd()用于尾部补全。

#### 堆栈溢出解决方法：
- 尾调用优化
  - 只有严格模式才开启。ES6模块默认开启严格模式
- 尾递归
- 蹦床函数

数组的扩展
------------

#### 扩展运算符
spread是三个点，和rest参数的逆运算一样
没有理解的想法
```
let 让arrayLike = {
    '0': ：'a',，
    '1': ：'b',，
    '2': ：'c',，
    length: 3长度：3
};

 // ES5的写法
var arr1 = [] .slice.call(（arrayLike);
```

暂时先到这里吧，先去学vue开发美团了，用es6的时候继续看，或者两个一起学
-------------------
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MDA2MjUzNzYsMTk4Mjg3NzcwNl19
-->
# 全栈记录
纯前端已经过时，全栈才是目标

## let const
`let` 声明变量
1. 不能重复声明
2. 支持块级作用域
`const` 声明产量
1. 控制修改
2. 支持块级作用域

## 作用域
### 1. 传统ES5
仅支持一种方式，函数作用域
### 2. ES6
块级作用域
```js
{}

if () {

}

for () {

}
```
这种`{}`写法，都是块级作用域

## 解构赋值
{a:12, b: 5, c: 33}
[12, 5, 8]
```js
let {a, b, c} = {a:12, b: 5, c: 33}
let [c, a, d] = [12, 5, 8]
```
左边的类型和右边必须一致
右边必须得是个东西？
## 函数
### 1. 箭头函数
() => {}
有且只有一个参数，可以`item => {}`
有且只有一个语句，可以`item => item + 1`
修复了this的问题，其他语音不存在此问题
### 2. 参数展开
...参数展开必须是最后一个数
```js
let name = (a, ...arr) => {
  console.log(a, ...arr)
}
name(1, 2, 4, 6, 7, 8);
```
展开数组，就是把值全部拿出来一样
```js
let arr = [1, 2, 4, 6 ,9]
console.log(...arr)
```
## ES6 新增系统对象
### 1.Array
`map` 映射
```js
let arr = [5, 6, 7, 8, 9];
let item = arr.map(item => item <= 7);
console.log(item);
```
`forEach` 就是把for封装起来了。
`filter` 过滤，很简单就是把返回false的值剔除
```js
let arr = [5, 6, 7, 8, 9];
let item = arr.filter(item => item <= 7);
console.log(item);
```
`reduce` 返回一个值，减少？
```js
let arr = [5, 6, 7, 8, 9];
let item = arr.reduce((tem,item) => tem + item);
console.log(item);
```
### 2.String
字符串模板
```js
let name = '模板';
console.log(`字符串${name}`);
```
`startsWith`
获取字符串开头值
```js
let http = 'http://';
let https = 'https://';
console.log(http.startsWith('http://'));
console.log(https.startsWith('http://'));
console.log(https.startsWith('https://'));
```
`endsWith`
和上面类似，不过是从结尾开始获取

## JSON ES5.5新增对象
`stringify()`  将对象转换为标准JSON
```js
var obj = {
  a: 1,
  b: 2,
  c: 3
}
console.log(JSON.stringify(obj))
```
`parse()` 将JSON格式的字符串转换为对象
```js
var obj = '{"a": "1", "b": "2", "c": "3"}';
console.log(JSON.parse(obj));
```

## 异步处理
### 回调地狱
```js
$.ajax({
  url: 'data/1.json',
  dataType: 'json',
  success(data1){
    $.ajax({
      url: 'data/2.json',
      dataType: 'json',
      success(data2){
        $.ajax({
          url: 'data/3.json',
          dataType: 'json',
          success(data3){
            console.log(data1, data2, data3);
          }
        });
      }
    });
  }
})
```


### `Promise` 创建一个promise
```js
let a = true
let prm = new Promise((resolve, reject) => {
  if (a) {
    resolve('success')
  } else {
    reject('err')
  }
})
prm.then(data => {
  console.log(data);
}, err => {
  console.log(data);
})
```
就是一个解决回调问题的处理方法，异步当然也可以，不过是一个统一的解决方案
`Promise.all([])`
```js
let p1 = new Promise((resolve, reject) => {
  resolve('success')
}),
p2 = new Promise((resolve, reject) => {
  resolve('success1')
}),
p3 = new Promise((resolve, reject) => {
  resolve('success2')
});
Promise.all([
  p1,
  p2,
  p3
]).then(([a, b, c]) => {
  console.log(a, b, c);
})
```
有一个promise抛出reject就会报错
如果里面是异步的，则会同时请求，请求完成取决于最慢的哪一个，这样的话我们就没办法第二个异步调用第一个返回的结果，这种问题我们可以用then解决呀。。。好像可以

### async/await
这个语法糖处理异步用的，ES8推出的
```js
let as = async () => {
  let s = 1;
  let s2 = 2;
  let data = await Promise;
}
```
`await` 后面必须是一个promise，不知道会不会自己转换。

## 语法糖
就是，写是这么写，到计算机真正执行的时候，会把函数编译回以前的写法，就比如我们的`async`
我们其实这么写，代码执行的时候还是一层套一层的回调，其实就是避免写起来很麻烦的尴尬局面
