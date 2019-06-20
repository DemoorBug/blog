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
null 和 undefined
never
object
类型断言
