---
title: javascript
date: 2019-01-13 08:55:59
tags:
categories:
---

# Array
**concat()**
用于连接两个或多个数组
**copyWithin()** es6
复制元素到指定位置
`array.copyWithin(复制到指定目标索引,[起始位置，[停止位置]])`
```js
var arrs = [1, 2, 3, 4, 5]
arrs.copyWithin(2,0,2)
```
**entries()** es6
给数组添加键值对,返回一个可迭代对象
`没有参数`
```js
var arrs = [1, 2, 3, 4, 5];
for (let i of arrs.entries()) {
  console.log(i)
}
```
**every()** 
和some() 一直，不过有不同，some位置下面有写
```js
let ages = [30,32,43,52];
ages.every((age) => {
  return age > 1
})
```
如果全部满足条件就返回true，负责一个不满足就会终止返回false
**filter()**
过滤，可以直接使用`every`的案例，结果就是过滤后的值
```js
let ages = [30, 32, 43, 52];
ages.filter((age) => age < 31) //[30]
```
**find()** es6
接收函数，检测条件为true时，直接返回，接下来就不会继续执行
```js
let ages = [30, 32, 43, 53];
ages.find((age) => age == 32) //32

```
**findIndex()** es6
返回索引，没有就是-1
```js
let ages = [30, 32, 43, 53];
ages.findIndex((a) => return a == 30) //结果返回索引0
```
**fill()**
填充
```js
let arrs = [30, 32, 43, 52];
arrs.fill('i', 0, 1)
```
**forEach()**
循环
```js
let ages = [30, 32, 43, 52];
ages.forEach((age, index) => console.log(age, index))
```
**from()** es6
处理带有length属性的对象，或可迭代对象，返回数组
```js
Array.from([2, 1, 3, 5, 6], (arr) => arr + 1) 
//案例中数组用new Set([2, 1, 3, 5, 6])这种方式，Set还没看，不清楚
```
**includes()** es6
感觉像是find的简单用法？这个方法可以替代`indexOf()`
```js
let ages = [30, 32, 43, 52];
ages.includes(30) //true,这个是es6方法，比indexOf强大
```
**indexOf()**
返回查询的值，没有就是-1，好处在于以前的浏览器兼容
```js
let ages = [30, 32, 43, 52];
ages.indexOf(30) //返回索引位置0，没有就是-1
```
**isArray()**
判断对象是否为数组
```js
let ages = [30, 32, 43, 52];
Array.isArray(ages) //true

```
**join()**
把数组转换为字符串，接收一个参数，通过什么符合组合
```js
let ages = [30, 32, 43, 52];
ages.join('') //30324352
```

**keys()** es6
将数组转换为可迭代对象(Iterator)，可以通过for...of处理
```js
let ages = [30, 32, 43, 52];
for (let i of ages.keys()) {
  console.log(i) //0, 1, 2, 3返回索引
}
```
**lastIndexOf()**
不常用，就是从最后一个开始检索字符串
```js
indexOf() //的反转用法
```
**map()**
感觉和`from()`很类似
```js
let ages = [30, 32, 43, 52];
ages.map((age) => age + 1) //[31, 32, 44, 53]
```
**pop()**
删除数组最后一个元素，并返回删除了那个元素，改变原数组
```js
let ages = [30, 32, 43, 52];
ages.pop() //52
```
**push()**
数组末尾添加元素
```js
let ages = [30, 32, 43, 52];
ages.push(23) //返回数组length 5
```
**reduce()**
他可以办到的forEach也可以，用这个逼格？还有[高级用法](https://www.jianshu.com/p/e375ba1cfc47)
```js
let ages = [30, 32, 43, 52];
ages.reduce((total, age, index, arr) => total+age) //结果为30+32+43+52总和
```
**reduceRight()**
和`reduce`是一样的，不过是从末尾开始
**reverse()**
颠倒数组顺序
```js
let ages = [30, 32, 43, 52]; 
ages.reverse() //修改原数组
```
**shift()**
删除并返回数组的第一个元素，和`pop()`兄弟
```js
let ages = [30, 32, 43, 52];
ages.shift() //返回删除的值30，改变原数组
```
**slice()**
截取数组并返回新的数组，不改变原数组
```js
ages.slice(2) //从第二个值开始截取，
```
**some()**
和`every()`区别在于，every有一个条件不符直接退出，some会查找到返回true为止，否则停止
**sort()**
可以让数组顺序依据字母顺序排序，默认升序，文档后面介绍很详细
```js
let ages = ['c', 'a', 'd', 'b'];
ages.sort() //还可以添加规则，数字的排序好像还要处理，字母的降序可以用reverse()
```
**splice()**
添加或删除，改变原数组
```js
let ages = [1, 2, 3, 4, 5];
ages.splice(2, 1, '5','1');   //[1, 2, '5', '1', 4 , 5]
```
**toString()**
将数组转换为字符串，返回结果
```js
let ages = [1, 2, 3, 4, 5];
ages.toString(); //1,2,3,4,5 join()更灵活
```
**unshift()**
数组开头添加  是push的兄弟
**valueOf()**
返回原始数组，用于调试



