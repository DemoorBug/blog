---
title: Programming
date: 2022-02-14 03:57:35
tags: [typescript] 
categories: [javascript, typescript] 
---

# 常用算法
map() 对范围中每个值应用一个函数,并返回应用该函数的结果
```javascript
map(["apple", "orange", "peach"], (item) => item.length)
// [5,6,5]
```
filter() 对范围中的每个值应用谓词(?),过滤掉谓词返回false的那些值
```javascript
filter(["apple", "orange", "peach"], (item) => item.length == 5)
// ["apple", "peach"]
```
reduce() 使用给定函数合并范围中的值, 并返回一个值
```javascript
reduce(["apple", "orange", "peach"], "", (acc, item) => acc + item)
// ""初始值, "appleorangepech" 结果值
// acc是累积项, 从初始值开始,最后得到所有元素合并后的值
["apple","orange", "peach"].reduce((acc, item) => acc+item, "123")
// array.reduce(callback, initiaValue)
```
> 上面这种用法不对,应该是得自己封装吧

> 在高层面上,编译器或解析器将把我们编写的源代码转换成机器(或运行时)能够理解的指令. 当运行时是一台物理计算机时,转换的指令将是CPU指令; 当运行时是虚拟机时, 则有自己的指令集和工具 

> 类型的主要优点在于正确性、不可变性、 封装、 可组合性、和可读性. 这5种优点时优秀的软件设计和行为的根本特性. 系统中总有出息混乱或者无序状态的倾向, 而上述特性则起到抗衡这种倾向的作用.

# 不可变性
```ts
// 1.4
function safeDivide(): nubmer {
  let x: number = 42
  // 使用常量 const x: number = 42
  if (x == 0) throw new Error("x should not be 0");
  x = x - 42
  // 修改常量会导致编译错误
  return 42 / x //除零导致无穷大
} 
```
变量被另一个并发执行的线程修改,或者被另外调用的函数悄悄修改. 一旦值发生变化, 执行检查的保证就不再有效, 如果这里使用常量就不会出现这个问题
> 从内存表示的角度来说,可变与不可变x没有区别. 常量性只对编译器有意义, 它是类型系统启用的一个属性
当涉及并发时,不可变性特别有用,因为如果数据不可变,就不会发生数据竞争

# 封装
封装值当是隐藏代码内部机制的功能,这里的代码可以是函数、类或者模块
我们利用封装, 是因为它可以帮助我们处理复杂性: 将代码拆分为更小的组件, 每个组件只向外界公开严格的需要的项, 而其他实现细节则被隐藏并隔离起来
```ts
// 1.6 封装程度不足
class SafeDivisor {
  divisor: number = 1
  // private divisor: number = 1
  // 变为私有成员,外部调用修改的时候ts就不会编译
  setDivisor(value: number) {
    if (value == 0) throw new Error("value should not be 0")
    this.divisor = value
  }
  divide(x: number): number {
    return x / this.divisor
  }
}

let sd = new SafeDivisor()
sd.divisor = 0 // 因为除数成员是公有的,所以检查可能被绕过修改
sd.divide(42)
```
public和private成员的内存表示是一样的

封装或信息隐藏使我们能够将逻辑和数据拆分到一个公有接口和一个非公有实现中. 在大型系统中, 这张拆分非常有帮助, 因为使用接口(或抽象)使理解一段特定代码的作用变得更加简单. 我们只需要理解组件的借口, 而不必理解其全部实现细节. 封装也有助于将非公有信息限制在一个边界内, 并保证外部代码不能修改这些信息,因为他们根本就访问不了这些信息.

封装出现在多个层次,例如,服务将其API公开为接口, 模块导出其接口并隐藏实现细节, 类只公开公有成员, 等等. 与嵌套娃娃一样, 代码两部分之间的关系越弱, 共享的信息就越少. 这样一来, 组件对其内部管理的数据能够做出的保证就得到了强化, 因为如果不经过该组件的接口, 外部代码将无法修改这些数据

# 可组合性
```ts
// 1.8 不可组合的系统
function findFirstnegativeNumber(number : number[]): number | undefined{
  for (let i of numbers) {
    if (i < 0) return i
  }
  // console.error("No matching value found")
}

function findFirestOneCharacterString(string: string[]): string | undefined {
  for (let str of strings) {
    if (str.length == 1) return str
  }
  // console.error("No matching value found")
}
```
假设有一个新的需求: 当在不到满足条件元素时,就记录一个错误,那就需要更新两个函数, 这就很麻烦了.

```ts
// 1.10 可组合系统
function first<T>(
  range: T[],
  p: (elem: T) => boolean
): T | undefined {
  for (let elem of range) {
    if (p(elem)) {
      console.log(elem);
      return elem
    }
  }
}
function findFirstNegativeNumber(
  number: number[]
): number | undefined {
  return first(number, n => n < 0)
}
function findFirestOneCharacterString(
  string: string[]
): string | undefined {
  return first(string, n => n.length == 1)
}
```
在第10章讨论泛型算法和迭代器的时候将会看到, 我们可以使用这个函数变得更加通用. 目前, 它只操作某个类型的T的一个数组. 可以扩展这个函数, 让它遍历人意数据结构

# 类型系统的类型
静态类型在编译时检查类型, 所以当完成变以后, 运行时的值一定有正确的类型. 另一方面, 动态类型则将类型检查推迟到运行时, 所以类型不匹配就成了运行时错误
强类型系统只会做很少的(甚至不做)隐士类型转换, 而弱类型系统则允许更多隐式类型转换
javascript时动态类型, typescript 时静态类型
> 动态类型不会在编译时施加任何类型约束. 日常交流中有时会将动态类型叫做”鸭子类型” (duck typing), 这个名称来自俗语: “如果一个动物走起来像鸭子, 叫起来像鸭子,那么它就是一只鸭子”
ts中使用any关键字可以模拟动态类型

js是弱类型,ts是强类型,`==`运算符在ts中无法编译,会直接报错

# 空类型 & 单元类型
函数在所有代码路径上都抛出异常(throw new Error('error'))、程序可能执行无限循环, 或者可能导致程序崩溃. 这些都算空类型`never`,如果用`void`就会存在误导性. 这些函数不是不返回有意义的值, 而是根本不返回

单元类型是只有一个可能值的类型. 对于这种类型变量, 检查其值是没有意义的, 它只能是哪一个值. 当函数的结果没有意义时, 我们会使用单元类型`void`

两个取值类型. 大多数语言都提供了布尔类型, 这是一个标准的、 只有两个值的类型

# 数值类型的常见陷阱
0.10+0.10+0.10 !== 0.30
[https://gauravkk22.medium.com/why-0-1-0-2-0-3-is-false-in-js-mystery-unsolved-with-solution-4f7db2755f18](为什么0.1+0.1+0.1不等于0.3)
```ts
// 2.10
function epsilonEqual(a: number, b: number): boolean {
  return Math.abs(a-b) <= Number.EPSILON
}
console.log(epsilonEqual(0.1+0.1+0.1, 0.3)) // true
// 因为0.3和0.30000000000000004在彼此的圆整误差内

```

# 编码文本
处理文本最好用库, grapheme-splittes,能够处理自负的书写位
上面这个库可帮助避免在处理字符串时最常出现的三类错误:
在字符串级别而不是书写位级别操纵编码文本
在字节级别而不是字符级别操纵编码文本
采用错误的编码来将一个字节序列解释为文本,例如试图将UTF-16编码文本解释为UTF-8文本,或者反过来解释
> 以前有门课程就专门说过不要直接使用(忘了是那个方法,substring?splice?)拆分文本,也没说啥原因,应该就是这种情况

UTF-8 8位==1个字节==取决于字符(英文1, 中文3, unicode 2)
但是uft-8是可变字节和uft-16一样,utf-8中的中文是3个字节,而unicode是两个字节

UTF-32 32位==4字节==1个字符
所有

# 组合
元祖类型: 元祖类型由一组类型构成,通过它们在元祖中的位置可以访问这些组成类型. 元祖提供了一种特殊的分组数据的方式, 允许我们将不同的类型的多个值作为一个值进行传递
> 元祖可以精确到每一个元素的类型

记录类型: 记录类型与元祖类型相似, 可将其他类型组合在一起. 但是, 元祖中安装分量值的位置来访问值, 而在记录类型中, 无名可以为分量设置名称, 并通过名称来访问值. 在不同语言中, 记录类型被称为记录或者结构

> 这里感觉用记录类型很麻烦,挺奇怪的,估计后面会讲type Point = {}?

枚举类型: 枚举类型的一个变量可以是提供的值和任何一个.每当我们有一小组可能的取值, 并且想要以不会导致歧义的方式表示他们时,就可以使用枚举

可选类型: Ts中用`|`来表示
```ts
// 3.12 可选类型
class Optional<T> {
  private value: T | undefined;
  private assigned: boolean;
  constructor(value?: T) {
    if (value) {
      this.value = value;
      this.assigned = true;
    } else {
      this.value = undefined
      this.assigned = false
    }
  }
  hasvalue(): boolean {
    return this.assigned
  }

  getValue(): T {
    if (!this.assigned) throw Error()
    return <T>this.value // 这是什么玩意啊? 不知道这样写意义何在
  }
}

```


Either类型, 见 3.14.ts
在发生错误时抛出异常,这是返回结构或错误的一个有效例子: 函数要么返回一个结果,要么抛出一个异常. 在某些情况下, 不能使用异常, 所以优先选择使用Either类型, 例如: 当在进程间或线程间传播错误时; 作为一种设计原则, 当错误本身算不上异常时(通常发生在处理用户输入的情况); 当调用的操作系统的API, 而这些API使用错误码时, 等等. 在这些情况中, 我们不能或不希望抛出异常,而是行为表达我们成功获得了值或者失败了, 此时最好把这种情形编码成"值或错误", 而不是"值和错误"
```ts
// Either
class Either<L, R> {
  private readonly value: L | R
  private readonly left: boolean
  private constructor(value: L | R, left: boolean) {
    this.value = value
    this.left = left
  }
  isLeft(): boolean {
    return this.left
  }
  getLeft(): L {
    if (!this.isLeft()) throw new Error()
    return <L>this.value
  }
  isRight(): boolean {
    return !this.left
  }
  getRight(): R {
    if (!this.isRight()) throw new Error()
    return <R>this.value
  }
  static makeLeft<L, R>(value: L) {
    return new Either<L, R>(value, true)
  }
  static makeRight<L, R>(value: R) {
    return new Either<L, R>(value, false)
  }
}

enum InputError {
  NoInput,
  Invalid
}
enum DayOfWeek {
  Sunday,
  Monday,
  TuesDay
}

type Result = Either<InputError, DayOfWeek>

function parseDayOfWeek(input: string): Result {
  if (input == "") Either.makeLeft(InputError.NoInput)
  switch (input.toLowerCase()) {
    case "sunday":
      return Either.makeRight(DayOfWeek.Sunday);
    case "sunday":
      return Either.makeRight(DayOfWeek.Monday);
    case "sunday":
      return Either.makeRight(DayOfWeek.TuesDay);
    default: 
      return Either.makeLeft(InputError.Invalid)
    
  }
}

```

变体类型: 变体类型也称为标签联合类型, 包含任意数的基本类型的值. 标签指的是即使基本类型有重合的值,我们仍能够准确说明该值来自那个类型.

原来typescript也可以这么难

# 类型安全
火星探测项目的类型约束
```ts
declare const NsType: unique symbol
class Ns {
  readonly value: number
  [NsType]: void
  constructor(value: number) {
    this.value = value
  }
}

declare const LbfsType: unique symbol
class Lbfs {
  readonly value: number
  [LbfsType]: void
  constructor(value: number) {
    this.value = value
  }
}

function LbfsToNs(lbfs: Lbfs): Ns {
  return new Ns(lbfs.value * 4.4482216152605)
}

function trajectoryCorrenction(momentum: Ns){
  if (momentum.value < new Ns(2).value) {
    console.log('momentum is too low'));
  }
}

function provideMonmentum() {
  trajectoryCorrenction(LbfsToNs(new Lbfs(1.5)))
  // 如果这里没有Ns传入值,就会报错,比如1.5
  // 必须经过Ns才行,例: new Ns(1.5)
}
```
一般来说,构造函数不应该做太繁重的工作, 而应该初始化对象的成员
private一个构造函数,就只能用工厂函数调用
```ts
// 工厂实施约束
declear const celsiusType: unique symbol
// typescript中用`unique symbol`来确保有相同形状的其他对象不会被解释为这个类型的一种方式
class Celsius {
  readonly value: number
  [celsusType]: void
  private constructor(value: number) {
    this.value = value
  }
  static makeCelsius(value: number): Celsius | undefined {
    if (value < -273.15) return undefined
    return new Celsius(value)
  }
}
```