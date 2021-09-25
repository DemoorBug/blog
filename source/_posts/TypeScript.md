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

# 项目开始
安装命令
```bash
npx create-react-app q --TypeScript //未来会弃用，使用下面的命令
npx create-react-app q --template typescript
```

安装了classnames 插件，还要安装附属的typescript
```bash
yarn add classnames -S
yarn add @types/classnames -S
```

代码方面倒是挺简单，样式是直接复制的
```ts
type NativeButtonProps = BaseButtonProps & React.ButtonHTMLAttributes<HTMLElement>
type AnchorButtonProps = BaseButtonProps & React.AnchorHTMLAttributes<HTMLElement>
export type ButtonProps = Partial<NativeButtonProps & AnchorButtonProps>;
// ButtonHTMLAttributes，AnchorHTMLAttributes button和a元素所有的附加属性，写起来还
// 会有代码提示, 用起来效果很NICE
// 此块代码的位置在项目的Button组件里面 q/src/components/Button/button.tsx:24
// Partial<T> 把里面的属性全部变成可选属性，难道以前的属性不是可选吗？(经测试，以前的属性都是可选的，所以这个算是多余代码，没必要。当然老师的意思估计是了解把)
// 为什么不直接一行解决？例如：
// type AllButtonProps = Partial<BaseButtonProps & React.ButtonHTMLAttributes<HTMLElement> & React.AnchorHTMLAttributes<HTMLElement>>
// 可以一行解决，还要写这么多，估计是好理解，确实
```


**测试**
测试框架有很多，这次用到的是一个后起之秀`JEST`

create react app 用的就是这个测试工具
已经内置了jest测试工具
```bash
npx jest jest.test.js //用于测试
npx jest jest.test.js --watch //一直运行，保存一次运行一次
```

ReactTestUtils 可搭配测试框架使用，比如jest，
官网推荐使用React Testing Library 也算是后起之秀
也可以使用Enzyme

create react app 3.0已经内置了React Testing Library了

三种文件会被自动认为是测试文件

- 在__tests__文件夹下面的.js，.ts文件
- 使用.test.js结尾的文件
- 使用.spec.js结尾的文件

**jest dom**
增加新的dom断言,这个库也已经内置了

插曲：设置npm代理(不上代理，总感觉很慢)，同时yarn没有任何设置，但是执行命令也是走的代理，挺奇怪的
```bash
npm config set proxy http://127.0.0.1:10809
npm config set https-proxy http://127.0.0.1:10809
// 第二句不设置也行，暂时没出问题，但是还是设置上为好
// 而且要配置,不然sudo执行命令的适合，不会使用代理
sudo npm config set proxy http://127.0.0.1:10809
sudo npm config set https-proxy http://127.0.0.1:10809
// 删除代理
npm config delete proxy
npm config delete https-proxy
// 同理sudo也要删除一遍
```
linux | ubuntu [设置全局代理](https://zhuanlan.zhihu.com/p/58690128)

```bash
# 修改shell配置文件 ~/.bashrc ~/.zshrc等
export http_proxy=socks5://127.0.0.1:1024
export https_proxy=$http_proxy

# 设置setproxy和unsetproxy 可以快捷的开关
# 需要时先输入命令 setproxy
# 不需要时输入命令 unsetproxy
alias setproxy="export http_proxy=socks5://127.0.0.1:1024; export https_proxy=$http_proxy; echo 'HTTP Proxy on';"
alias unsetproxy="unset http_proxy; unset https_proxy; echo 'HTTP Proxy off';"
# 设置快捷开关这个倒是不错的想法，不过目前没有应用场景，用上倒是可以借鉴
```

nvm因为不能sudo执行，安装的node也不能用sudo命令访问，要通过软链接才行

```bash
# $NVM_DIR 这些常量是.nvm的目录，后面的$(nvm version)，版本号，挺讨巧的
sudo ln -s "$NVM_DIR/versions/node/$(nvm version)/bin/node" "/usr/local/bin/node"
sudo ln -s "$NVM_DIR/versions/node/$(nvm version)/bin/npm" "/usr/local/bin/npm"
```

[sudo时无法使用代理](https://blog.csdn.net/u014789266/article/details/76571763)

```bash
#在/etc/sudoers中，env_reset下添加
Defaults env_keep="http_proxy https_proxy ftp_proxy no_proxy
```





上次看的是jest测试，还是结合react的测试更有趣，让我知道了用处

```ts
import React from 'react'
import { render, fireEvent } from '@testing-library/react'
import Button, { ButtonProps, ButtonSize, ButtonType}  from './button'
// jest.fn() 创建出一个被监控的模拟函数

const defaultProps = {
  onClick: jest.fn()
}
// 这里用 ButtonProps 是因为会有代码提示，判断什么类型很方便
const testProps: ButtonProps = {
  btnType: ButtonType.Primary,
  size: ButtonSize.Large,
  className: 'names',
}

const disabledProps: ButtonProps = {
  disabled: true,
  onClick: jest.fn()
}
// 分类测试
describe('test Button component', () => {
  // test 简写也可以是it
  it('should render the correct default button', () => {
    const wrapper = render(<Button {...defaultProps}>Nice</Button>)
    const element = wrapper.getByText('Nice') //测试文本
    expect(element).toBeInTheDocument() //判断组件是否在文档中
    expect(element.tagName).toEqual('BUTTON') //判断元素是否为BUTTON
    expect(element).toHaveClass('btn btn-default') //判断class 是否含有btn btn-default
    fireEvent.click(element) // 模拟点击
    expect(defaultProps.onClick).toHaveBeenCalled() // 判断是否处罚点击事件
  })
  it('should render the correct component based on different props', () => {
    const wrapper = render(<Button {...testProps}>Nice</Button>)
    const element = wrapper.getByText('Nice')
    expect(element).toBeInTheDocument()
    expect(element).toHaveClass('btn-primary btn-lg names')
  })
  it('should render a link when btnType equals link and href is provided', () => {
    const wrapper = render(<Button btnType={ButtonType.Link} href="http//www.google.com">Link</Button>)
    const element = wrapper.getByText('Link')
    expect(element).toBeInTheDocument()
    expect(element.tagName).toEqual('A')
    expect(element).toHaveClass('btn btn-link')
  })
  it('should render disabled button when disabled set to true', () => {
    const wrapper = render(<Button {...disabledProps}>Nice</Button>)
    const element = wrapper.getByText('Nice') as HTMLButtonElement // 返回一个HTMLElement，但是我们要用到disabled，而且我们认为必返回Button，所以我们可以使用类型断言，把他变成一个button
    expect(element).toBeInTheDocument()
    expect(element.disabled).toBeTruthy()
    fireEvent.click(element)
    expect(disabledProps.onClick).not.toHaveBeenCalled() // 因为是disabled属性，所以这里用到了not，表示没有被点击
  })
})

```

------

## 插曲：批量更改文件名，shell脚本

文件结构

```
  大秧歌
    - 01.mkv
      - 01.mkv.xls
```
IDM/shell.sh:
```shell
#!/bin/bash

for l in `ls 大秧歌`
  do
  mv 大秧歌/$l/`ls 大秧歌/$l` 大秧歌压缩/`ls 大秧歌/$l | sed 's/....$//' | sed 's/^/大秧歌./'`
done
```
优化代码:
```shell
#!/bin/bash
mkdrname='大秧歌'

for l in `ls $mkdrname`
  do
  mv $mkdrname/$l/`ls $mkdrname/$l` $mkdrname压缩/`ls $mkdrname/$l | sed 's/....$//' | sed 's/^/'$mkdrname'./'`
done
```



如果是直接命令输出脚本，则要用echo关键字
比如大秧歌目录下执行命令如下:
```bash
for s in `ls`; do mv $s `echo $s | sed 's/^/大秧歌./'`; done
```

处理带有空格文件的时候会循环错误，一个文件有几个空格就循环几次

```bash
for folder in `ls|tr " " "?"`
 do
  # 如果所目录的情况
  folder=${folder//'?'/' '}

 cd "$folder"

done

 

## 先把空格用特殊符号代替，然后替换即可。  使用cd时需要添加双引号
```

> 先把空格用特殊符号代替，然后替换即可。使用cd时需要添加双引号
>
> [PS](https://blog.csdn.net/dqswuyundong/article/details/7427467)

改进后

处理带有空格的文件名

```bash
for s in `ls|tr " " "?"`; do mv "$s" `echo $s | sed 's/..$//'`; done
```

> 研究了半天，终于搞懂了



***

## 图标Icon解决方案



font 字体已经过时

svg 可以采用

[Font Awesome](https://fontawesome.com/icons?d=gallery&m=free)

[react-fontawesome](https://github.com/FortAwesome/react-fontawesome)

```tsx
import { fas } from '@fortawesome/free-solid-svg-icons'
import { library } from '@fortawesome/fontawesome-svg-core'
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'
library.add(fas)
// fas代表了引入所有图标，然后就可以很轻松的用字符串代替了
<FontAwesomeIcon icon='coffee'/>
// 或者是这样
import { faCoffee } from '@fortawesome/free-solid-svg-icons'
<FontAwesomeIcon icon='faCoffee'/>
```

[Sass的each循环](https://sass-lang.com/documentation/at-rules/control/each)

创建一个`With Maps`

```scss
// _variables.scss
$theme-colors: 
(
  "primary": $primary,
  "secondary": $secondary,
  "success": $success,
  "info": $info,
  "warning": $warning,
  "danger": $danger,
  "light": $light,
  "dark": $dark
)

// Icon/_style.scss
@each $key, $val in $theme-colors {
  .icon-#{$key} {
    color: $val;
  }
}

```

**React中制作动画**

动画间隙，这个东西具体叫什么忘记了，不过vue学过，忘了，行吧，不过这东西react的挺简单的

[官网插件推荐](https://reactjs.org/docs/faq-styling.html#can-i-do-animations-in-react)

[react-transition-group这个库用的最多](https://reactcommunity.org/react-transition-group/)

```bash
yarn add react-transition-group
yarn add @types/react-transition-group 
这个typescript的插件官网没有介绍
```

我人傻了，居然是classNames,我写成className 导致了子组件的className被替代，我还以为组件升级写法变了，研究半天，莫名其妙

```tsx
<CSSTransition
    in={menuOpen}
    timeout={300}
    classNames='zoom-in-top' // 致命错误classNames 不是 className
    appear
    >
    <ul className={subMenuClasses}>{childrenComponent}</ul>
</CSSTransition>
```

莫名其妙的报错，findDomNode

[React官方的解释，没看懂](https://reactjs.org/docs/strict-mode.html#warning-about-deprecated-finddomnode-usage)

react-transition-group的解决方法是4.4.0 换种写法

挺奇怪的，为什么官方不自己封装一下，肯定有其他用处

```tsx
fucntion SubMenu (){
	const nodeRef = React.useRef(null)
	return (
        <CSSTransition nodeRef={nodeRef}>
        	<div ref={nodeRef}></div>
        </CSSTransition>
    )
}
```



## storybook

[storybook.js.org](storybook.js.org)

```bash
npx sb init
安装不成功就换个节点。前面几次都没成功，换了个节点好了
```

```bash
git diff
查看变化，Q退出
```

真是醉了，搞了半天的styorbook 的props显示问题，老师解决起来很麻烦，因为用的旧版本，新版本直接集成了，谁知道居然也这么麻烦，官方文档也没提，导出的时候不能`export default`导出，不然就会显示寥寥无几的`props`，直接导出组件就行了`export`,还有就是默认值的问题，如果用元组`enum`的话，就会导致默认值是一个变量，很不好理解，目前解决办法就是用`type`声明，不用元组就ok了



## 自动完成input

```jsx
// 忽略值
Omit<InputHTMLAttributes<HTMLElement>, 'size'>
// 目前用到的场景，继承的时候这个值和当前接口size冲突，必须改名或者这种解决
// omit删除inputHTMLAttributers中的size属性
export interface InputProps
  extends Omit<
    React.InputHTMLAttributes<HTMLElement>,
    'size'
  > {
  disabled?: boolean
  size?: 'lg' | 'sm'
  icon?: IconName
  prefixs?: string | React.ReactElement
  append?: string | React.ReactElement
  theme?: ThemeProps
}
因为size属性和input默认属性冲突，所以这里要忽略
```

[Omit<>](https://www.typescriptlang.org/docs/handbook/utility-types.html#omittype-keys)

**input 表单，优化** 课程9-3节，对应文件路径src/components/Input/*.tsx

`onChange`如果是input表单上面用，会导致`e`参数是HTMLElement类型，自己增加一个类型检测就行了

```jsx
// input.tsx
export interface InputProps extends Omit<React.InputHTMLAttributes<HTMLElement>, 'size'> {
    onChange?: (e: React.changeEvent<HTMLInputElement>) => void
}
```

受控组件 [Controlled Components](https://reactjs.org/docs/forms.html#controlled-components) 

我的理解是有value，就是受控组件，defaultValue就是非受控组件

如果自己写的组件让别人用，同时输入以上两个参数，就会报错，所以我们必须优化代码，其实我觉得没必要

```tsx
// input.stories.tsx
import React from 'react'
export const Input: React.FC<InputProps> = (props) => {
	const {...restProps} = props
    if ('value' in props) { // 如果有value这个值
        delete restProps.defaultValue // 就删除defaultValue
    }
    return <><input {...restProps}></>
}
```

如果使用的时候state传入参数为`undefined`或者`null`也会上述报错，`undefined`

被识别为非受控组件

使用：

```jsx
// input.stories.tsx
const inputTemplate: Story<InputProps> = args => {
    const [value, setValue] = useState() //如果这里是空，参数类型就会变成`undefined`
    return (
    	<Input {...args} value={value} onChange={e => setValue(e.target.value)} /> // 这样赋值就会报错，因为value类型是undefind，而e.target.value类型是string, 其二 页面改变input值也会warning非受控组件被修改问题
    )
}
```

修复：

```tsx
// input.tsx
const valueDefault = (value: any) =>  {
    if (value === 'undefined' || value === null) {
        return ''
    }
    return value
}
if('value' in props) {
    delete restProps.defaultValue 
    restProps.value = valueDefalt(restProps.value) //主要代码是这里
}
```

## 测试的一个小bug

menu.test.tsx中94行*expect(wrapper.queryByText('drop1')).not.toBeVisible()*报错，理所应当，因为必须要鼠标事件才会显示，以前不知道为什么没发现，写完没测试？？不应该啊

后面还有一个报错[
waitFor error "MutationObserver is not a constructor" with latest version](https://github.com/testing-library/dom-testing-library/issues/477#) 这个原因是jsdom依赖的jest版本低，可以用一个很麻烦的方法解决，另一个就是更新react-scripts，和jest来解决，我用的第二种

```bash
yarn upgrade react-script@^
	- 4.0.1  # 目前的最新版,此版本依赖jest 26.6.0
yarn upgrade jest@^
	- 26.6.0
查看版本以及依赖，npm ls jsdom

// 上面这种更新用法，会导致package不更新，遗留问题？，我好像遇到了，后面用，又消失了这个bug
```

发现一个新写法，还挺好看+简洁源文件写法：src\components\Input\input.tsx

```tsx
  const cnames = classNames('viking-input-wrapper', {
    [`input-size-${size}`]: size,
    'is-disabled': disabled,
    'input-group': prepend || append,
    'input-group-append': !!append,
    'input-group-prepend': !!prepend
  })
  // !!双感叹号，是因为append是string类型，一个感叹号变为布尔值，另一个感叹号变为真正的true还是false，有点绕口，不知道该怎么解释。
```













































------

> 这里是另一个ts教程笔记，弃用
>

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
