---
title: TypeScript
date: 2019-06-11 20:59:54
tags: [TypeScript]
categories: 学习
---

TypeScript 学习
[库地址](https://github.com/DemoorBug/Typescript-axios)
<!-- more -->

换一套课程，上一个就是按照官方文档来的

# 2-4基础类型
undefined 和 null是所有类型的子类型

比如：
```ts
let num: number = undefined;
let num: number = null;
```

跳过检查`any`
```ts
let strOrnum: any = 'num'
strOrnum = 213
```
联合类型`|`
```ts
let strOrnum: number | string = 'num'
strOrnum = 2123
```
数字数组`number[]`
```ts
let arrOfNumbers: number[] = [1, 2, 3]
arrOfNumbers.push(5)
arrOfNumbers.push('6') //报错，必须是数字
```

类数组
```ts
function test() {
  console.log(arguments);
  //arguments 就是一个类数组
  // 可以用arguments.length
  // 但是没有数组的方法
  let arr: any[] = arguments
  //这样会报错
}
```
元组
```ts
let user: [string, number] = ['viking', 123]
```
**interface 接口**
> 对对象的形状(shape)进行描述
> 对类(class) 进行抽象
> Duck typing (鸭子类型)

```ts
interface Person {
  name: string;
  age: number;
}
let viking: Person = {
  name: 'viking',
  age: 18
}
```

可选属性`?`
```ts
interface Person {
  name: string;
  age?: number;
}
```
只读属性`readonly`
```ts
interface Person {
  readonly id: number;
  name: string;
  age?: number;
}
// readonly 是加在前面
let obj: Person = {
  id: 0,
  name: 'string',
  age: 18
}

obj.id = 2 // 报错，不可修改

```
> const 和 readonly的区别是，const是用在变量上面，readonly是用在属性上面

**函数**
函数的构成，输入，和返回

```ts
// 函数声明
function add(x:number, y: number): number {
  return x + y
}

let result = add(2, 3)
```
可选参数
```ts
function add(x:number, y: number, z?: number): number {
  if (typeof z === 'number') {
    return x + y + z
  }
  return x + y
}
// 可选参数，必须是最后一个，不能更改顺序
```
函数表达式
```ts
const add = function add(x:number, y: number): number {
  return x + y
}
const add2: (x: number, y: number, z?: number) => number = add

// 出现的`=>`不是ES6的箭头函数，而是ts提供的写法
```
**类 class**
类(Class): 定义了一切事物的抽象特点
对象(Object): 类的实例
面向对象(OOP): 三大特性: 封装、继承、多态
封装: 数据操作细节隐藏，仅暴露对外的接口，外界调用端不需要知道细节，就可以用对外提供的接口来访问对象
继承: 子类继承父类，子类除了拥有父类的所有特性外，还有一些更加具体的特性
多态: 是由继承而产生的相关的不同的类，对同一个方法呢可以有不同的响应，比如猫和狗都继承自动物,但是他们分别实现了自己吃的方法，此时针对某一个实例，我们无需了解他是猫还是狗，我们可以直接调用吃方法，程序就会自动判断出如何正确执行吃方法

```ts
class Animal {
  name: string;
  constructor(nam: string) {
    this.name = nam
  }
  run() {
    return `${this.name} is runing`
  }
}

// 实例化
const snake = new Animal('snake')

class Dog extends Animal {
  bark() {
    return `${this.name} is barking`
  }
}

const xiaobao = new Dog('xiaobao')
console.log(xiaobao.run());
console.log(xiaobao.bark());

// 重写run
class Cat extends Animal {
  constructor (name: string) {
    super(name)
    console.log(this.name);
  }
  run () {
    return 'Meow, ' + super.run()
  }
}
const maomao = new Cat('maomao')
console.log(maomao.run());

//多态：我们不用关注调用run的时候他是猫还是狗，系统会自动判断执行正确方法
```
类里面用到的修饰符

默认是 `public`
有些属性或方法，不愿意对外访问就要用到
`private` 只能在类里面访问，子类都不能访问
如果希望子类可以访问，就要用到
`protected`
如果想让属性只能访问不能修改，可以用
`readonly`
静态属性(ES6也有？),不用实例也可以访问
```ts
class Animal {
  static categoies: string[] = ['mammal','bird']
}
```
静态方法
```ts
class Animal {
  static isAnimal(a) {
    return a instanceof Animal
  }
}
const snake = new Animal('li')

console.log(Animal.isAnimal(snake));

```

**类和接口**
```ts
interface Radio {
  switchRadio(): void
}
interface RadioWithBattery extends Radio {
  checkBatteryStatus(): void
}

class Car implements Radio {
  switchRadio() {

  }
}

class Cellphone implements RadioWithBattery {
  switchRadio() {

  }
  checkBatteryStatus() {

  }
}
```
interface 接口
对对象的形状(shape)进行描述
对类(class)进行抽象
Duck Typing(鸭子类型)

> 鸭子类型：只要你走起来像鸭子,叫起来像鸭子，我就不管你是什么东西，我就可以用它来约束各种概念上毫不相关内容

**枚举**
数字枚举
```ts
enum Direction {
  Up,
  Down,
  Left,
  Right
}
```
常量枚举
```ts
const enum Direction {
  Up,
  Down,
  Left,
  Right
}

```
枚举分两种，只有常量值可以常量枚举，计算值不能使用常量枚举不可以加const，上面讲的都是常量值，计算值后面用到了讲

**泛型(最难的部分)**
动机，要解决什么问题(太NICE了，比那个好多了)
```ts
function name(num:number): number {
  return num
}
const sr = name(123);
// 如果我们要传入字符串、布尔、复杂类型，就要使用到any，那样就会导致sr常量就会丧失类型，泛型就是解决这种问题的
```
泛型: 是指在定义函数接口或类的时候，我们不预先指定具体类型，而是在使用的时候再指定类型的一种特征
老师讲的很通透，感觉挺简单？
```ts
function names<T>(num:T): T {
  return num
}
const sr = names('123')
// 现在不管我们传入什么，ts类型推断都会帮我们推断出常量的类型
function names<T, U>(params:[T, U]): [U, T] {
  return [params[1], params[0]]
}
const sr = names(['number', 123]) //类型推断 [number, string]
```

更深入的用法
只能传入数组
```ts
function echoWithArr<T>(arg: T[]): T[] {
  arg.length
  return arg
}
const arr = echoWithArr([1, 2]) //只能传入数组
```
这种方法就可以传入带length属性的类型参数
```ts
interface Length {
  length: number
}
function echoWithArr<T extends Length>(arg: T): T {
  arg.length
  return arg
}
const str = echoWithArr('str')
const obj = echoWithArr({length: 1})
const arr2 = echoWithArr([1, 2, 3])
```
在类里面使用泛型
为什么要用到
```ts
class Queue {
  private data = []
  push(item) {
    return this.data.push(item)
  }
  pop() {
    return this.data.shift()
  }
}
const queue = new Queue()
queue.push(1)
queue.push('srt')

console.log(queue.pop().toFixed());
console.log(queue.pop().toFixed());
// 这里就会报错，因为第二个值是string，string没有toFixed方法
// 简单修改
class Queue {
  private data = []
  push(item: number) {
    return this.data.push(item)
  }
  pop(): number {
    return this.data.shift()
  }
}
// 这样也可以，如果需求传入更多参数，会变得很麻烦
```
使用泛型类
```ts
class Queue<T> {
  private data = []
  push (item: T) {
    return this.data.push(item)
  }
  pop(): T {
    return this.data.shift()
  }
}
const queue = new Queue<number>()
queue.push(1)

console.log(queue.pop().toFixed());
```
interface 泛型
```ts
interface KeyPair<T, U> {
  key: T;
  value: U;
}

let kp1: KeyPair<number, string> = {
  key: 123,
  value: 'str'
}

```
泛型数组
```ts
let arr: number[] = [1, 2]
let arr: Array<number> = [1, 2]
```
泛型描述函数
```ts
interface IPlus {
  (a: number, b: number): number
}

function plus(a:number, b: number): number {
  return a + b
}

const a: IPlus = plus
```
泛型interface 结合泛型描述函数
```ts
interface IPlus<T> {
  (a: T, b: T): T
}

function plus(a:number, b: number): number {
  return a + b
}

const a: IPlus<number> = plus
```
**类型别名**
// type aliases
```ts
type PlusType = (x: number, y: number) => number
function sum(x: number y:number): number {
  return x + y
}
const sum2: PlusType = sum

```
如果一个函数要接受参数比较复杂，比如联合类型接收一个函数，就要用到类型别名
```ts
type NameResolver = () => string
type NameOrResolver = string | NameResolver
function getName(n: NameOrResolver): string {
  if (typeof n === 'string') {
    return n
  } else {
    return n()
  }
}
```
**类型断言**
// type assertion
```ts
function ls(s: number | string) {
  // s.length 这样会报错，因为联合类型number没有length
  (s as string).length
  //这样就ok了
  (<string>s).length
  // 好像也没有比as简单。。
}
```
**声明文件**
后缀为.d.ts的文件ts都会解析
如果用到jQuery的话，直接写会报错，要声明
```ts
declare var jQuery: (selector: string) => any
```
tsconfig.json 配置文件
```json
  {
    "include" : ["**/*"]
  }
  // 这个字段就是告诉编译器，帮我们编译当前文件夹下的所有文件
```
官方也为我们准备好了声明文件
比如`@type/jquery`就是jquery的声明文件

[查看声明文件的一个网址](https://microsoft.github.io/TypeSearch/)

**React + typescript**
**ts组件，太好用了**
```ts
import React from 'react'

interface IHelloProps {
  message?: string;
}
// const Hello = (props: IHelloProps) => {
//   return <h2>{props.message}</h2>
// }

// React.FC是React.FunctionComponent的别名
// React.FC 内置了很多方法，很有用，目前就讲了defaultProps
// ts可以直接替代以前的propTypes(类型检查)
const Hello: React.FC<IHelloProps> = (props) => {
  return <h2>{props.message}</h2>
}

Hello.defaultProps = {
  message: 'Hello word'
}

export default Hello
```

**自定义hook**
必须use开头

高阶组件HOC就是一个函数，接受一个组件作为参数，返回一个新的组件




——————————————————————————————————————

# 安装
```bash
npm i typescript -g
tsc -V
```

# 基础类型
布尔值
```typescript
let isDone: boolean = true
```
数字
```TypeScript
let decLiteral: number = 20
```
字符串
```typescript
let name: string = 'bob'
name = 'smith'
```
数组
```TypeScript
// 这两种写法等价
let list: Array<number> = [1, 2, 3]
let list: number[] = [1, 2, 3]
```
元组 Tuple
```typescript
let x: [string, number]
x= ['hello', 10]
```
枚举
```typescript
enum Color {
  Red,
  Green,
  Blue
}

// let c: Color = Color[2]
let c: string = Color[2]

console.log(c)
```
any
```typescript
let notSure: any = 4
notSure = 'maybe a string instead'
notSure = false
```

```typescript
let list: any[] = [1, true, 'free']
list[1] = 100
```
void

```typescript
function warnUser(): void { //没有返回值的函数
  console.log('This i my waring message')
}
```
null 和 undefined
```typescript
let u: undefined = undefined
let n: null = null
```
never 永远不存在
```typescript
function error(message: string): never {
    throw new Error(message);
}
function fail() {
  return error('something faild')
}
```
object
```typescript
declare function create(o: object | null) : void;
//原始类型 非原始类型
create(o: {prop: 0})
create(o: null)
//基础类型 不行
create(o: 42)
create(o: 'string')
create(o: false)
create(o: undefined)
```
类型断言
```typescript
let someValue: any = 'string'
// let strLength: number = (<string>someValue).length
let strLength: number = (someValue as string).length
```

---
```typescript
let num: number = 3
num = null
```
编译
```bash
tsc greeter.ts // 这么是不会报错的
tsc greeter.ts --strictNullChecks //这么编译就会出错，就要用到联合类型，就不会报错了
```
联合类型
```typescript
let num: number | null = 3
num = null
```

# 接口

```typescript
interface Square {
  color: string
  area: number
}
```
可选参数
```typescript
interface SquareConfig {
  color: string
  width?: number
}
```
完整实例
```typescript
interface Square {
  color: string
  area: number
}

interface SquareConfig {
  color?: string
  width?: number
}

function createSquear(config: SquareConfig): Square {
  let newSquear = { color: 'white', area: 100 }
  if (config.color) {
    newSquear.color = config.color
  }
  if (config.width) {
    newSquear.area = config.width
  }
  return newSquear
}

console.log(createSquear({color: 'red'}))
```
只读属性,创建完成之后就不能再改变了
```TypeScript
interface Point {
  readonly x: number
  readonly y: number
}

let p1: Point = { x: 10, y: 20 }

p1.x = 2 // 报错，尝试对一个只读属性修改
```
泛型只读数组
```typescript
let a: number[] = [1, 2, 3, 4]
let ro: ReadonlyArray<number> = a
ro[0] = 12 // 编译报错
a = ro as number[] // 这样可以，类型断言
```
额外属性检查
索引签名
```typescript
// 这段代码要和上面的完整实例结合使用
interface SquareConfig {
  color?: string
  width?: number

  [propName: string]: any // 索引签名
}

console.log(createSquear({color: 'red', colorus: 'names'}))
```
函数类型接口
```typescript
interface SearchFunc {
  (source: string, subString: string): boolean
}

let mySearch: SearchFunc
mySearch = function (src, sub) {
  let result = src.search(sub)
  return result > -1
}

// 不使用接口实现
mySearch = function (source: string, subString: string): boolean {
  let result = src.search(sub)
  return result > -1
}
```

索引签名
```ts
interface StringArray {
  [index: number]: string
}

let myArray: StringArray

myArray = ['Bob', 'Fred']

let myStr: string = myArray[0]
```


# 遇到的问题，搞不懂，先记录下来

同时拥有string和number类型的索引签名，number索引返回值必须是string索引返回的子类型，因为当用一个数字索引时，js实际会在索引到对象之前将其转换为字符串


```ts
interface Animal {
  name: string;
}

interface Dog extends Animal {
  breed: string;
}

// Error: indexing with a numeric string might get you a completely separate type of Animal!
interface NotOkay {
  [x: number]: Animal;
Numeric index type 'Animal' is not assignable to string index type 'Dog'.
  [x: string]: Dog;
}
```
