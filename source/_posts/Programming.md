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
# 问题
我的那个pubg网站的遥测数据应该如何存储以及读取,
目前解决办法就是最最简单的数组吧,
如果别人快进呢?

# 异构集合
包含相同集合的项,称之为:同构集合
反之包含不同类型集合的项,称之为:异构集合

# 序列化
```ts
// 4.4.2
class Cat {
  meow(){}
}
class Dog {
  bark(){}
}
function serializeCat(cat: Cat): string {
  return "c" + JSON.stringify(cat)
}
function serializeCat(dog: Dog): string {
  return "c" + JSON.stringify(dog)
}

function tryDeserializeCat(from: string): Cat | undefined {
  if (from[0] != "c") return undefined
  return <Cat>Object.assign(new Cat(), JSON.parse(from.substr(1)))
}

let catString: string = serializeCat(new Cat())
let dogString: string = serializeDog(new Dog())

let maybeCat: Cat | undefined = tryDeserializeCat(catString)

if (maybeCat != undefined) {
  let cat: Cat = <Cat>maybeCat
  cat.meow()
}
maybeCat = tryDeserializeCat(dogString)
// 这里返回undefined

```
# 函数类型
函数类型和签名: 函数的实参类型和返回类型决定了函数的类型. 如果两个函数接受相同的实参, 并返回相同的类型, 那么它们具有相同的类型. 实参集合加上返回类型也称为函数的签名

一等函数: 将函数复制给变量, 并像处理类型系统中的其他值一样处理它们, 就得到了所谓的一等函数. 这意味着将函数视为“一等公民”, 赋予它们与其他值相同的权利: 它们有类型, 可被赋值给变量, 可作为实参传递, 可被检查是否有效 , 以及在兼容情况下可被转换为其他类型

1. 将一个可以处于打开（open）或关闭（closed）状态的简单连接建模为一个状态机。使用connect函数打开连接，使用disconnect关闭连接。
```ts
// 函数状态机
class Connection {
  private doProcess: () => void = this.connect
  public switchControl() {
    this.doProcess()
  }
  private connect() {
    this.doProcess = this.disonnect
    console.log('closed')
  }
  private disonnect() {
    this.doProcess = this.connect
    console.log('open')
  }
}
```
```ts
// 简单状态机
enum ControlProcessingMode {
  Open,
  Closed
}
class ControlProcessing {
  private mode: ControlProcessingMode = ControlProcessingMode.Open
  public processIs() {
    this.processLine()
  }
  private processLine(): void {
    switch (this.mode) {
      case ControlProcessingMode.Open:
        this.processOpen()
        break
      case ControlProcessingMode.Closed:
        this.processClosed()
        break
    }
  }
  processOpen() {
    this.mode = ControlProcessingMode.Closed
    console.log('closed')
  }
  processClosed() {
    this.mode = ControlProcessingMode.Open
    console.log('open')
  }
}

```

1. 我们可以把连接建模为状态机，它有两个状态和两个转移。这两个状态为open和closed，这两个转移为从closed转移到open的connect转移，以及从open转移到closed的disconnect转移。

使用process()方法将前面的连接实现为一个函数状态机。对于关闭的连接，process()打开一个连接；对于打开的连接，process()调用read()函数，后者返回一个字符串。如果该字符串为空，则关闭连接；否则，将读取的字符串输出到控制台。read()函数的声明如下：declare function read(): string;。

```ts
// 函数状态机
declare function read(): string
class Connection {
  private doProcess: () => void = this.processClosedConnection
  public process(): void {
    this.doProcess()
  }
  private processClosedConnection() {
    console.log('closed')
    this.doProcess = this.processOpenConnection
  }
  private processOpenConnection() {
    console.log('open')
    const value: string = read()
    if (value.length == 0) {
      this.doProcess = this.processClosedConnection
    } else {
      console.log(value)
    }
  }
}
```
# 装饰器
装饰器模式是一个简单的行为软件设计模式,可扩展对象的行为,而不必修改对象的类. 装饰的对象可以执行其原始实现没有提供的功能
WidgetFactory装饰器
```ts
class Widget {}
interface IWidgetFactory {
  makeWidget(): Widget
}
class WidgetFactory implements IWidgetFactory {
  public makeWidget(): Widget {
    return new Widget()
  }
}
class SingletonDecorator implements IWidgetFactory {
  private factory: IWidgetFactory
  private instance: Width | undefined = undefined
  constructor(factory: IWidgetFactory) {
    this.factory = factory
  }
  public makeWidth(): Width {
    if (this.instance == undefined) {
      this.instance = this.factory.makeWidget()
    }
    return this.instance
  }
}
```

函数式小部件工厂
```ts
class Width {}
type WidgetFactory = () => Widget
function makeWidget(): Widget {
  return new Widget()
}
function use10Widget(factory: WidgetFactory) {
  for(let i = 0; i < 10; i++) {
    let width = factory()
    /* ... */
  }
}

use10Widget(makeWidget)
```
函数式小部件工厂装饰器
```ts
class Widget {}
type WidgetFactory = () => Widget
function makeWidget(): Widget {
  return new Widget()
}
function singletonDecoratoy(factory: WidgetFactory): WidgetFactory {
  let instance: Widget | undefined = undefined
  return (): Widget => {
    if (instance == undefined) {
      instance = factory()
    }
    return instance
  }
}
function use10Widgets(factory: WidgetFactory) {
  for(let i = 0; i < 10; i++) {
    let widget = factory()
    /* ... */
  }
}
use10Widgets(singletonDecorator(makeWidget))
```
现在`use10Widgets()`不会构造10个Widget对象, 而是会调用Iambda(匿名函数), 为所有调用重用相同的Widget实例

习题
```ts
class Widget {}
type WidgetFactory = () => Widget
function loggingDecorator(factory: WidgetFactory): WidgetFactory {
  return () => {
    console.log("Widget created")
    return factory()
  }
} 

```

可恢复函数: 可恢复的函数是跟踪自己状态的函数, 在被调用时, 不会从头开始运行, 而是从上一次返回时所在的状态恢复执行
在ts中,这种函数不实用return,而是用yileld关键字,这个关键字会刮起函数,将控制权交给调用者. 当再次被调用时, 将从yileld语句恢复执行
使用yield有另外两个限制: 必须把函数声明为一个生成器(函数加*声明生成器), 并且其返回类型必须是可迭代的迭代器
可恢复的计数器
```ts
function* counter(): IterableItertor<number> {
  let n: number = 1
  while (true) {
    yield n++
  }
}
let counter1: iterableIterator<nubmer> = counter()
console.log(counter1.next())
// 1
console.log(counter1.next())
// 2
console.log(counter1.next())
// 3
```
闭包实现
就是匿名函数调用外部变量,导致自己不会被摧毁,形成闭包
```ts
type count = () => number 
function counter(): count {
  let i = 0
  return () => {
    return i++
  }
}
const coun: count = counter()
console.log(count())
console.log(count())
console.log(count())
console.log(count())
```

# 安全反序列化
any类型很危险,因为不仅可以把任何值赋值any类型, 还可以把any值赋值给其他任何类型, 从而绕过类型检查

unknown和any的区别: 尽管我们可以把任意值赋值给unknown和any,但是使用这两种类型的变量时, 存在一个区别. 对于unknwon的情况, 只有当我们确认一个值具有某个类型(如User)时, 才能把该值用作该类型. 对于any的情况, 我们可以立即把该值用作其他任何类型的值. any会绕过类型检查
```ts
class User {
  name: string
  constructor(name: string) {
    this.name = name
  }
}
// 如果把unknown换成any,就可以直接使用greet(),而不用做类型判断isUser()
function deserialize(input: string): unknown {
  return JSON.parse(input)
}

function greet(user: User): void {
  console.log(`Hi ${user.name}`)
}
// 如果user是true,则类型是User,否则类型是any
function isUser(user: any): user is User {
  if (user === null || user === undefined) {
    return false
  }
  return typeof user.name === 'string'
}

// 如果把unknown换成any,就可以直接使用greet(),而不用做类型判断isUser()
let user: unknown = deserialize('{"name": "John"}')
if (isUser(user)) {
  greet(user)
}
user = undefined
if (isUser(user)) {
  greet(user)
}

```
never
底层类型: 如果一个类型时其他类型的子类型, 那么我们称之为底层类型, 因为它位于子类型层次的地段. 要成为其他类型的子类型, 它必须具有其他类型的成员. 因为我们可以有无限个类型和成员, 所以底层类型也必须有无限个成员. 这是不可能发生的, 所以底层类型始终是一个空类型: 这是我们不能为其创建实际值的类型
由于never是底层类型, 所以我们能把它赋值给其他任何类型, 这也意味着我们能够从函数中返回该类型. 由于这是向上转换(将某一个值从子类型转换为父类型), 可被隐式完成,因此编译器不会报错.
```ts
enum TurnDirection {
  Left,
  Right
}
function fail(message: string): never {
  console.error(message)
  throw new Error(message)
}
function turnAngle(turn: TurnDirection): number {
  switch (trun) {
    case TurnDirection.Left: return -90
    case TurnDirection.Right: return 90
    default: return fail("unknown TurnDirection")
  }
}

```
```ts
type triangle {
  name: 'triangle'
}
type Square {
  square: 'square'
}
type Circle {
  circle: 'circle'
}
type All = triangle | Square | Circle
/*
*/
type select = triangle | Square
/*
*/
```
名义和结构子类型: 在名义子类型中, 如果显式声明一个类型是另一个类型的子类型, 则二者构成子类型关系. 在结构子类型中, 如果一个类型具有另一个类型的所有成员, 并且可能还有其他成员, 那么前者是后者的子类型

每当我们使用继承时, 得到的子类型比父类型的属性多. 对于和类型, 则是相反的: 父类型比子类型的类型更多
```ts

declare const TriangleType: unique symbol
class Triangle{
  [TriangleType]: void
}
declare const SquareType: unique symbol
class Square{
  [SquareType]: void
}
declare const CircleType: unique symbol
class Circle{
  [CircleType]: void
}
declare const EquilateralTrangleType: unique symbol
class EquilateralTriangle extends Triangle {
  [EquilateralTrangleType]: void
}
/*
class EquilateralTriangle {
  [EquilateralTriangleType]: void
  [Triangle]: void
}
*/
/*
如果是这样就不能编译:
declare function makeShape(): Triangle | Square;
// 因为父类型里面必须包含EquilateralTriangleType
// 但是makeShape中没有这个类型,所以就不能直接赋值,反之则可以
declare function draw(i: EquilateralTriangle | Square | Circle): void;

draw(makeShape())
*/
declare function makeShape(): EquilateralTriangle | Square;
declare function draw(i: Triangle | Square | Circle): void;
/*
这样则可以编译
父元素并没有规定必须要有EquilateralTriangle
而是规定必须有Triangle, EquilateralTriangle中继承了Triangle所以包含
*/

draw(makeShape())

```

子类型和集合
```ts

declare const ShapeType: unique symbol
class Shape {
  [ShapeType]: void
}
declare const TriangleType: unique symbol
class Triangle extends Shape {
  [TriangleType]: void
}
class LinkedList<T> {
  value: T
  next: LinkedList<T> | undefined = undefined
  constructor(value: T) {
    this.value = value
  }
  append(value: T): LinkedList<T> {
    this.next = new LinkedList(value)
    return this.next
  }
}
declare function makeTriangles(): LinkedList<Triangle>
declare function draw(shapes: LinkedList<Shape>): void

draw(makeTriangles())
```
协变: 如果一个类型保留其底层类型的子类型关系, 就称该类型具有协变性. 数组具有协变性, 因为它保留了子类型关系: Triangle是Shape的子类型, 所以triangle[]是Shape[]的子类型

当处理数组和集合(如LinkedList<t>)时, 不同的语言具有不同的行为. 例如,在C#中,必须通过声明接口并使用out关键字(ILinkedList<out T>), 显式指出一个类型(如LinkedList<T>)的协变. 否则, 编译器将不会推断出子系列类型关系

不变性: 如果一个类型不考虑其底层类型的子类型关系, 就称该类型具有不变形. C#中的List<T> 具有不变形, 因为它不考虑子类型关系“Trangle是Shape的子类型”, 所以list<Shape>和List<Triangle>之间不存在子类型-父类型关系

子类型和函数的返回类型
```ts
// 7.18

declare const ShapeType: unique symbol
class Shape {
  [ShapeType]: void
}
declare const TriangleType: unique symbol
class Triangle extends Shape{
  [TriangleType]: void
}
declare function factory (): Shape
declare function makeTriangle(): Triangle
declare function makeShape(): Shape
function useFactory(Factory: () => Shape): Shape {
  return factory()
}
let shape1: Shape = useFactory(makeShape)
let shape2: Shape = useFactory(makeTriangle)
```
函数的返回类型具有协变性. 
# 子类型和函数实参类型
```ts
declare const ShapeType: unique symbol
class Shape {
  [ShapeType]: void
}
declare const TriangleType: unique symbol
class Triangle extends Shape {
  [TriangleType]: void
}
declare function drawShape(shape: Shape): void
declare function drawTriangle(triangle: Triangle): void
function render(
  triangle: Triangle,
  drawFunc: (argument: Triangle) => void
): void {
  drawFunc(triangle)
}
render(new Triangle, drawShape)
// 当期望接收到一个(argument: Triangle) => void
// 可以使用(argument: Shape) => void
```
这个逻辑上时合理的: 我们有一个Triangle,并将其传递给一个可以把它用作实参的绘制函数. 如果函数自身期望收到一个Triangle, 如这里的drawTriangle()函数, 那么这当人可以工作. 但是, 对于期望收到Triangle的父类型函数, 它也应该可以工作. drawShape()希望绘制任何一个形状. 因为它没有使用任何三角形特有的数据, 所以比drawtriangle()更具一般性; 它可以接受任何形状作为实参, 无论这个形状时Triangle还是Shape. 因此, 在这种情况下, 子类型关系反过来了

逆变: 如果一个类型颠倒了其底层类型的子类型关系, 则称该类型具有逆变性. 在大部分编程语言中, 函数的实参上逆变的. 一个接受Triangle作为实参的函数可以被替换为一个接受Shape作为实参的函数. 函数之间的关系与其实参类型之间的关系相反. 如果Triangle是Shape的子类型, 那么接受Triangle作为实参的函数类型是接受Shape作为实参的函数的类型的父类型

在Typescript中, 如果Triangle是Shape 的子类型, 那么函数(argument: Shape) => void和函数类型(agumengt: Triangle) => void 能够彼此替换. 它们实际上互为子类型. 这种属性称为双变形
双变形: 如果类型的底层类型的子类型关系决定了它们互为子类型, 则称这种类型具有双变形. 在Typescirpt中, 如果Triangle是Shape的子类型, 那么函数类型(argument: Shape) => vodi 和 (arugment: Triangle) => void互为子类型

同样,在Typescript中, 函数实参的双变形可能导致错误的代码通过编译. 本书的一个重要主题是依赖类型系统, 在编译时消除运行时错误. 在Typescript中, 支持常用的Javascript编程模式是有意做出的设计决策
习题:
1. 可以
```ts
declare const ShapeType: unique symbol
class Shape {
  [ShapeType]: void
}
declare const TriangleType: unique symbol
class Triangle extends Shape {
  [TriangleType]: void
}
declare function drawShape(shape: Shape): void
drawShape(new Triangle)
```
2. 否
```ts
declare const ShapeType: unique symbol
class Shape {
  [ShapeType]: void
}
declare const TriangleType: unique symbol
class Triangle extends Shape {
  [TriangleType]: void
}
declare function drawShape(shape: Triangle): void
drawShape(new Shape)
```
3. 是 集合和数组具有协变性
4. 可以 逆变,互为子类型
5. 可以
6. 不行

卧槽,全对,没毛病哈,哈哈

# 实用接口定义契约
面向对象编程(Object-Orientend Programming, OOP): OOP是基于对象的概念的一种编程范式, 对象可以包含数据和代码. 数据是对象的状态, 状态是一个或多个方法, 也叫做“消息”. 在面向对象系统中, 通过实用其他对象的方法, 对象之间可以 “对话”或者发送消息
OOP的两个关键特征是封装和继承. 封装允许隐藏数据的方法, 而继承则使用额外的数据和代码扩展一个类型

哈,抽象类?`abstract`关键字
```ts
// 抽象日志系统实现
abstract class ALogger {
  abstract log(line: string): void
}
class ConsoleLogger extends ALogger {
  log(line: string): void {
    console.log(line)
  }
}
```
```ts
//日志系统接口实现
interface ILogger {
  log(line: sring): void
}
class ConsoleLogger implements ILogger {
  log(line: string): void {
    console.log(line)
  }
}
```
这两种方法很相似,并且都可以工作, 但是在这样的一种场景中, 我们应该使用接口, 因为接口指定了一种契约
接口和契约: 接口(或契约)描述了实现该接口的任何对象都理解的一组消息. 消息是方法, 包括名称、实参和返回类型. 接口没有任何状体. 与实现世界的契约(它们是书面协议)一样, 接口也相当于书面协议, 规定了实现者将提供什么

抽象类能够实现这种行为, 但还可以做更多工作: 抽象类可以包含非抽象的方法和状态. 抽象类和“普通”类, 或者说具体类的唯一区别在于, 我们不能直接创建抽象类的实例. 我们知道, 每当传递抽象类的一个实例(如ALogger实参),实际上是在实用ALogger派生类型(如ConsoleLogger)的实例

```ts
interface IterableIterator<T> extends Iterable<T> , Iterator<T>{

}
```
# 继承数据和行为
继承是面向对象语言最为熟知的特性之一, 允许创建父类的子类. 子类继承了父类的数据和方法. 显然, 子类是父类的子类型, 因为在期望使用父类的任何地方, 都可以使用子类的实例

继承和“是一个”关系: 继承会在子类型与父类型之间建立“是一个”关系. 如果基类是Shape, 派生类是Circle, 关系就是“Circle是一个Shape”. 这是继承在语义上的含义, 可用来判断是否应该在两个类型之间应用继承
例: Cat 是一个 Pet 是一个 Animal
```ts
class Animal {}
class Pet extends Animal {}
class Cat extends Pet {}
```
如果没有这层关系,就应该使用组合,而不是继承

# 组合数据和行为
面向对象编程的一个著名原则是, 只要可能, 就优先选择组合而不是继承. 接下来介绍组合
```ts
class Shape {
  id: string
  constructor(id: string) {
    this.id = id
  }
}
class Point {
  x: number
  y: number
  constructor(x: number, y: number) {
    this.x = x
    this.y = y
  }
}

class Circle extends Shape {
  center: Point
  redius: number
  constructor(id: string, center: Point, redius: number) {
    super(id)
    this.center = center
    this.redius = redius
  }
}
```
"有一个"经验准则
正如应用‘是一个‘测试可判断是否让Circle继承Point, 对于组合, 可以应用一个类似的测试: ’有一个‘

Circle(圆是一个形状,所以继承了ID属性) 是一个 Shape(形状) 所有形状都有一个ID
Circle(圆有一个圆心属性) 有一个 Point(点) 点有x和y坐标

适配器模式:
```ts
namespace Geometylibray {
  export interface Icircle {
    getCenterX(): number;
    getCenterY(): number;
    getDiameter(): number;
  }
}
type Circle = {
  center: {x: number, y: number};
  radius: number;
}
class CircleAdapter implements Geometylibray.Icircle {
  private circle: Circle
  constructor(circle: Circle) {
    this.circle = circle
  }
  getCenterX(): number {
      return this.circle.center.x
  }
  getCenterY(): number {
      return this.circle.center.y
  }
  getDiameter(): number {
      return this.circle.radius * 2
  }
}
```

混入扩展:
```ts
function extend<First, Second>(first: First, second: Second): First & Second {
  const result: unknown = {}
  for (const prop in first) {
    if (first.hasOwnProperty(prop)) {
      (<First>result)[prop] = first[prop]
    }
  }
  for (const prop in second) {
    if (second.hasOwnProperty(prop)) {
      (<Second>result)[prop] = second[prop]
    }
  }

  return result as First & Second 
}

class MeowingPet extends Pet {
  meow(): void {

  }
}
class HunterBehavior {
  track(): void {

  }
  stalk(): void {

  }
  pounce(): void {

  }
}
type Cat = MeowingPet & HunterBehavior;
const fluffy: Cat = extend(new MeoWingPet(), new HunterBehavior())
```

# 泛型数据结构
泛型类型: 泛型类型是指参数化一个或多个类型的泛型函数、类、接口等.泛型类型允许我们编写能够使用不同类型的通用代码, 从而实现高度的代码重用

```ts
class Box<T> {
  readonly box: T
  constructor(value: T) {
    this.box = value
  }
}
```

```ts
function unbox<T>(box: Box<T>) T {
  return box.box
}

```

9.2.3
```ts
class Stack<T> {
  private values: T[] = []
  push(value: T) {
    this.values.push(value)
  }
  pop(): T{
    if (this.values.length == 0) throw Error()
    return this.values.pop()
  }
  peek(): T {
    if (this.values.length == 0) throw Error
    return thsi.values[this.values.length - 1]
  }
}
```

```ts
class Pair<T, U> {
  readonly first: T
  readonly second: U
  constructor(first: T, second: U) {
    this.first = first
    this.second = second
  }
}

```
# 遍历数据结构
假设我们想按中序遍历二叉树打印其所有元素的值, 
中序遍历是指“左子结点-父结点-右子结点”的方式进行递归遍历
```ts
// 创建二叉树 & 遍历二叉树
class BinaryTreeNode<T> {
  value: T
  left: BinaryTreeNode<T> | undefined
  right: BinaryTreeNode<T> | undefined
  constructor(value: T) {
    this.value = value
  }
}

function printInOrder<T>(root: BinaryTreeNode<T>): void {
  if (root.left != undefined) {
    printInOrder(root.left)
  }
  console.log(root.value)
  if (root.right != undefined) {
    printInOrder(root.right)
  }
}

let root: BinaryTreeNode<number> = new BinaryTreeNode(1)
root.left = new BinaryTreeNode(2)
root.left.right = new BinaryTreeNode(3)
root.right = new BinaryTreeNode(4)
printInOrder(root)

//    1
//2      4 
//  3

// print: 2 3 1 4
```
打印链表:
```ts
class LinkedListNode<T> {
  value: T
  next: LinkedListNode<T> | undefined
  constructor(value: T) {
    this.value = value
  }
}
function printLinkedList<T>(head: LinkedListNode<T>): void {
  let current: LinkedListNode<T> | undefiend = head
  while (current) {
    console.log(current.value)
    current = current.next
  }
}
let head: LinkedListNode<string> = new LinkedListNode("hello")
head.next = new LinkedListNode("World")
head.next.next = new LinkedListNode("!!!")
printListedList(head)
```
迭代器: 迭代器是能够用来遍历数据结构的一个对象. 它提供一个标准呢接口, 将数据结构的实际形状对客户端隐藏起来