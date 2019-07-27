---
title: TypeScript
date: 2019-06-11 20:59:54
tags: [TypeScript]
categories: 学习
---

TypeScript 学习
<!-- more -->
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
