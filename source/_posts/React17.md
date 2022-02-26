---
title: React17
date: 2022-02-24 09:05:17
tags: ['react']
categories: [react, ts]
---
# 使用nvm灵活切换版本

```bash
nvm install node # node是最新版本的别名
#or
nvm install 10
nvim install 9
#or
nvm install 6.14.3 # 指定更加具体的版本
#or
nvm use node # 切换到最新版
nvm use 10 
nvm use 6.14.3
#or 
nvm alias default node # 这些命令可以看到具体版本
nvm alias default 10
nvm alias default 6.14.3
```

# react 元素如何工作
每当你调用createClass, 扩展Component, 或声明一个无状态函数, 你就在创建一个组件. react在运行时管理你的组件的所有实例, 而且在某一特定时间点,内存中可能有不止一个相同组件的实例 
react遵循声明式范式,不需要告诉它如何与dom交互,你想在屏幕上看到什么, react就会为你完成工作
为了控制ui流程, react使用了一种特殊的类型对象 , 称为元素, 它描述了屏幕上必须显示的内容. 与组件及其实例相比, 这些不可变的对象要简单的多, 只包含表示界面所需的严格信息
例:
```js
{
  type: Title,
  {
    color: 'red',
    children: 'Hello, Title!'
  }
}
```
type属性很重要, 它告诉react如何处理这个元素本身. 如果类型是一个字符串, 该元素代表一个DOM节点, 而如果是一个函数, 该元素是一个组件
DOM元素和组件相互嵌套, 以代表渲染树
```js
type: Title,
props: {
  color: 'red',
  children: {
    type: 'h1',
    props: {
      children: 'Hello, H1!'
    }
  }
}
```
当元素的类型是一个函数时, react会调用这个函数,通过props来获取底层元素. 它不断地对结果进行同样的操作, 直到得到一颗DOM节点树, 让react可以在屏幕上渲染. 这个过程被称为调和, 它让React Dom和React Native用来创建它们各自平台的UI

随着我们网络上工作经验的增加, 我们了解到不可能掌握所有东西, 我们应该找到正确方法来保持自己的更新,以避免疲劳. 我们能够跟随所有的新趋势, 而不是为了跳入新库而跳入新库, 除非我们有时间把他当一个副业.

在javascript世界里,一旦一个规范被宣布或起草, 社区里就会有人把它做成一个插件或一个polyfill来实现, 让其他都来玩, 而浏览器供应商则同意并开始支持它

与其他语言或平台相比, 这一点使得js和浏览器成为一个完全不同的环境. 它的缺点是, 变化的太快, 但它只是一个在押注新技术和保持安全之间找到正确的平衡

使用react需要学习数百种不同的工具是不正确的, facebook意识到大家已经感到疲劳, 所以发布了一个cli工具,使其令人难以置信的容易搭建和运行一个真正的react应用程序
```shell
npm install -g create-react-app 
create-react-app hello-world --template typescript

# npm老是报错,proxy错误,很烦
# 解决了, 给npmjs.org设置代理就会导致报错,离谱,通过设置代理白名单解决,为什么呢,还不让用代理

# yarn可以解决这个问题,太棒了
# 也不知道解决没有, 报错连接失败,但是成功构建了应用,而npm则抛出错误
yarn create create-react react17 --template typescript
# 这段代码等价于:
yarn add global create-react-app 
create-react-app react17 --template typescript
# 如果这样的话,岂不是和npm冲突?
```

# typescript
类型: 你可以随性所欲的命名类型, 但一个好的做法是添加一个T前缀, 这样一来, 你就能很快认出

接口与类非常相似, 有时开发人员不知道它们之间的区别. 接口可以用来描述一个对象的形状或函数签名, 就像类型一样, 但语法不同

使用接口的时候,命名最好带一个I,这样一来,你就能很快认出它是一个接口,而不会困惑的认为它是一个类或一个react组件

一个接口或类型可被扩展, 但同样的, 语法会有所不同, 如一下代码块所示
```ts
interface Iwork {
  company: string
  position: string
}
interface Iperson extends IWork {
  name: string
  age: number
}
type TWork = {
  company: string
  position: string
}
type TPRson = TWork & {
  name: string
  age: number
}
// 将一个接口扩展为类型
interface IWork {
  company: string
  position: string
}
type TPerson = IWork & {
  name: string
  age: number
}
```
一个类可以用完全相同的方式实现一个接口或类别名. 但是它不能实现(或扩展) 一个命名为联合类型的类型别名
```ts
interface IWork {
  company: string
  position: string
}
class Person implements IWork {
  name: 'Carlos'
  age: 33
}
// 类型
class Person2 implements TWork {
  name: 'Cristina'
  age: 32
}
// 这样使用会报错
// 不可以使用联合类型
type TWork2 = {
  company: string
  position: string
} | {
  name: string
  age: number
}
class Person3 implemenets Twork2 {
  company: 'Google'
  position: 'Senior Software Engineer'
}
```
与类型不同,一个接口可以被多次定义, 并将被视为一个单一的接口(所有的声明将被合并)
```ts
interface IUser {
  username: string
  email: string
  name: string
  age?: number
  website: string
  active: boolean
}
interface IUser {
  country: string
}
const user: IUser = {
  username: 'demoorbug',
  email: 'demo@gmail.com',
  name: 'Carlos Santanna',
  country: 'Mexico',
  age: 33,
  website: 'http://www.baidu.com',
  active: true
}
```
# Babel
我们需要安装@babel/core和@babel/node
@babel/cli 可以不用安装
```bash
npm install -g @babel/core @babel/node
# 编译
babel source.js -o output.js
```
Babel只是一个将源文件转译成输出文件的工具, 但要应用一些转换, 我们需要对它进行配置

有一些非常有用的预设配置,可以通过安装来使用
```bash
npm install -g @babel/preset-env @babel/preset-react
```
根目录创建.babelrc配置文件
```json
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}
```
这样我们就可以在源文件中编写ES6和JSX, 并在浏览器中执行输出文件

在React17中, React.createElement('div')被废弃了, 现在内部使用react/jsx-runtime来渲染jsx, 这意味我们会有诸如_jsx('div', {})的东西, 这意味着我们不再需要`import React from 'react'`的导入来编写jsx代码

```html
<img src="" 
alt="JS Education"
/>
```

```js
_jsx("img", {
  src: "",
  alt: "JS Education"
})
```
JSX允许你定义元素来描述元素树并组成复杂的UI. 
```html
<a href="https://***">Click me</a>
```
```js
_jsx(
  "a",
  { href: "https://***"},
  "Click me!"
)
```
更复杂的ui树
```html
<div><a href="https://***">Click me</a></div>
```

```js
_jsx(
  "div",
  null,
  _jsx(
    "a",
    { href: "https://***" },
    "Click me"
  )
)

```
JSX用类似xml语法让一切更加可读和维护, 但重要的是要知道, 与我们的jsx并行的javascript可以控制元素的创建. 好的方面是, 我们不局限于将元素作为子元素, 而是可以使用javascript表达式, 比如函数或变量, 为此, 我们必须将表达式用花括号包起来
```jsx
<div>
  Hell, {variable}
  I'm a {() => console.log('Function')}
</div>
```
任何变量和函数都应该用花括号包起来

因为jsx不是一种标准语言, 它被转译为javascript, 正因为如此,有些属性不能被使用

例如, 我们必须使用className来代替class, for用htmlFor代替,其原因是,class和for是javascript中的保留词(原来如此)

样式(style):
一个相当重要的区别是style属性的工作方式
样式属性并不像HTML并列那样接受css字符串, 而是期待一个javascript对象, 其中的样式名称是CamelCased:
```jsx
<div style={{ backgroundColor: 'red'}}>
```
你可以将一个对象传递给style, 这意味着如果你愿意, 甚至可以将你的样式放在一个单独的变量中
```js
const styles = {
  backgroundColor: 'red'
}
<div style={styles}>
```
值得一提的是, 与HTML有一个重要区别, 由于JSX元素被翻译成javascript函数, 而在javascript中你不能返回两个函数, 只要你在同一层上有多个元素, 你就不得不将它们包裹在一个父元素上

之前, react强迫你返回一个用<div>元素或任何其他标签包裹的元素; 自React 16.2.0以来, 可以直接返回一个数组
```jsx
return [
  <li key="1">First item</li>,
  <li key="2">Second item</li>,
  <li key="3">Third item</li>,
]
```
或者直接返回一个字符串
```js
return 'Hello World'
```
另外, react现在有一个性能叫Fragment, 它可以作为元素的一个特殊包装. 它可以用React.Fragment来指定
```js
import { Fragment } from 'react
return (
  <Fragment>
    <h1>An h1</h1>
    some
    <h1>An h1</h1>
    some
  </Fragment>
)
```
或者可以直接使用空标签(<></>)
```js
return (
  <>
    <ComponentA />
    <ComponentB />
    <ComponentC />
  </>
)
```
Fragment不会在Dom上渲染任何可见的东西; 它只是一个辅助标签, 用来包装你的React元素或组件

空格(Spaces)
我们应该始终牢记JSX不是HTML, 即使它有类似XML的语法. JSX处理文本和元素之间的空格的方式与HTML不同
```jsx
<div>
  <span>My</span> name is 
  <span>Carlos</span>
</div>
```
html中会渲染成My name is Carlosj
而在tsx中会渲染成: MynameisCarlos
解决方案(这也忒麻烦了):
```js
<div>
  <span>My</span>
  {' '}
  name is 
  {' '}
  <span>Carlos</span>
</div>
```
Boolean属性
在JSX中定义布尔属性的方式
如果你设置了一个没有值的属性,JSX会假定它的值为真, 这与HTML的disabled属性的行为相同, 例如
```js
<button disabled />
React.createElement("button", { disabled: true })
```
这在开始时令人困惑, 因为我们可能认为省略一个属性就意味着假, 但事实并非如此. 在React中, 我们应该明确表达, 以避免混淆

扩展属性操作符(...)(Spread attributes)
一个重要的扩展属性操作服(...), 它来自ES提案的rest/spread属性, 当我们想要将javascript对象的所有属性传递给元素时, 它非常方便. 

一个常见的做法是不通过引用将整个javascript对象传递给子对象, 而是使用它们的原始值, 这些值很容易验证, 使组件更加健壮和防止错误
```js
const attrs = {
  id: 'myId',
  className: 'myClass'
}
return <div {...attrs}>
```
这些代码会被编译成:
```js
var attrs = {
  id: 'myId',
  className: 'myClass'
}
return _jsx('div', attrs)
```
模版字符串(Template literals)
模版字符串时允许前乳表达式的字符串字元. 你可以用它们来使用多行字符串和字符串插值功能
模版字符串使用反斜线(``)字符包围, 而不是双引号或单引号, 此外, 模版字元可以包含占位符. 你可以使用美元符号和大括号添加它们(${expression})添加它们
```js
const name = 'Carlos'
const multilineHtml = `<p>This is a multiline string</p>`
console.log(`Hi, my name is ${name}`)
```

三元条件:
检查一个以上的变量来决定是否渲染一个组件,这种情况下使用內联条件是一个很好的解决方案, 但可读性会受到影响. 相反我们可以在组件內创建一个辅助函数, 并在JSX中使用它来验证该条件
```js
<div>
  {dataIsReady && (isAdmin || userHasPermission) && <SecretData />}
</div>
```
辅助函数:
```js
const canShowSecretData = () => {
  const { dataIsRedy, isAdmin, userHasPermission } = props
  return dataIsReady && (isAdmin || userHasPermissions)
}
return (
  <div>
    {this.canShowSecretData() && <SecretData />}
  </div>
)
```
正如我们看见的,这种改变使代码更易读, 条件更明确, 如果你在6个月后再看这段代码, 仅仅阅读函数的名称, 你任何会发现它很清晰

还有其他的解决方案,但是需要依赖外部库, 一个好的做法是尽量避免外部依赖, 以保持我们的包更小, 但在某些时候, 这可能是值得的, 因为提高我们模版的可读性是一个很大的胜利.
第一个解决方案是render-if, 我们可以通过一下方式安装
```bash
npm install -S render-if
```
```js
const { dataIsReady, isAdmin, userHasPermissions } = props
const canShowSecretData = renderIf(
  dataIsReady && (isAdmin || userHasPermissions)
)
return (
  <div>
    { canShowSecretData(<SecretData>)}
  </div>
)
```

一个目标是不要在我们的组件中添加太多的逻辑. 有些组件会需要一些逻辑, 但我们应该尽量让它们保持简单, 这样我们就可以很容易地发现和修复错误.

我们至少应该保持renderIf方法的简洁, 为了做到这一点, 我们可以使用另一个工具库, 叫做react-only-if, 它可以让我们通过使用高阶组件(HOC)设置条件函数, 将我们的组件写成条件永远为真
第四章中会广泛的讨论HOC, 但现在, 你只需要知道它们是接收一个组件并添加一些属性或修改其行为返回一个增强的组件的函数
```js
npm install -S react-only-if
```
使用
```js
import onlyIf from 'react-only-if'
const SecretDataOnlyIf = onlyIf(
  ({dataIsReady, isAdmin, userHasPermissions}) => dataIsReady && (isAdmin || userHasPermissions)(SecretData)
)

const MyComponent = () => {
  <div>
    <SecretDataOnlyIf
      dataIsReady={...}
      isAdmin={...}
      userHasPermissions={...}
    >
  </div>
}
export default MyComponent
```
正如你在这里看到的, 条件本身没有任何逻辑
我们把条件作为onlyif函数的第一个参数传递给它, 当条件被匹配时, 组件就渲染.
用来验证条件的函数接受组件的props、状态(state)和上下文(context)
通过这种方式, 我们避免了用条件式来污染我们的组件, 这样它就更容易理解和推理了.

条件和循环
条件和循环是UI模版中非常常见的操作,你可能会觉得使用javascript三元表达式或map函数来执行它们是错误的. JSX的构建方式是只对元素的创建进行抽象, 将逻辑部分留给真正的javascript, 这样做很好,只是有时, 代码会变得不那么清晰.
一般来说, 我们的目标是将所有的逻辑从我们的组件中移除, 特别是我们渲染的方法, 但有时我们必须根据应用程序的状态来显示和隐藏元素, 而且很多时候我们必须在集合和数组中循环
如果你觉得使用JSX进行这种操作会使你的代码更易读, 有一个Babel插件就可以做到这一点: JSX-control-statements
```bash
npm install -S jsx-control-statements
```
.babelrc
```json
"plugins": ["jsx-control-statements"]
```
使用该插件提供的语法
```js
<If condition={this.canShowSecretData}>
  <SecretData>
</If>
```
这将被转换为一个三元表达式
```js
{canShowSecretData ? <SecretData /> : null}
```
If组件很好,但如果由于某种原因,你的渲染中有嵌套条件,它就很容易变得混乱和难以遵循. 这时候就可以使用Choose组件
```js
<Choose>
  <When condition={...}>
    <span>if</span>
  </When>
  <When condition={...}>
    <span>else if</span>
  </When>
  <Otherwise>
    <span>else</span>
  </Otherwise>
</Choose>
```
请注意,这些代码都会被转写成多个三元表达式
最后, 有一个组件(永远记住我们不是在谈论真正的组件, 而只是语法糖)来管理循环, 这非常方便:
```js
<ul>
  <For each="user" of={this.props.users}>
    <li>{user.name}</li>
  </For>
</ul>
```
这些代码被转换成一个map函数,这没有什么神奇的

> 还要安装一个linters: eslint-plugin-jsx-control-statements

次级渲染(Sub-rendering)
值得强调的是, 我们总是希望保持我们的组件非常小, 我们的渲染方法非常干净和简单
然而, 这并不是一个简单啊目标, 尤其是当你在迭代创建一个应用程序时, 在第一次迭代中, 你并不清楚如何将组件分割成小的组件.那么 当渲染方法变得太大, 难以维护时, 我们应该怎么做? 一个解决方案是把它拆成更小的函数,让我们吧所有的逻辑放在一个组件中
```jsx
const renderUserMenu = () => {
  // JSX for user menu
}
const renderAdminMenu = () => {
  // JSX for admin menu
}
return (
  <div>
    <h1>Welcome back!</h1>
    {userExists && renderUserMenu()}
    {userIsAdmin && renderAdminMenu()}
  </div>
)
```
这并不是最佳方法, 因为将组件分割成小的组件似乎更明显. 然后, 有时它有助于保持渲染方法的整洁. 例如在Redux的实际例子中, 一个sub-render方法被用来渲染load more按钮

styling code(编码方式)
使用EditorConfig(或Prettier)和ESLint, 通过验证你代码风格来提高你的代码质量. 团队中避免使用不同风格代码

最初Douglas Crockford用JSLint(2002年)使linting在javascript中流行起来,最后,react 使用的是ESLint
ESLint是一个在2013年发布的开源项目, 其具有高度可配置性和可扩展性
安装eslint
```bash
npm i -g eslint eslint-config-airbhb eslint-config-prettier eslit-plugin-import eslint-plugin-jsx-ally eslint-plugin-prettier eslint-plugin-react
```
安装完毕后,可以用一下命令执行
```bash
eslint source.ts
```
输出文件将告诉我们文件中是否有错误
当我们第一次安装和运行它的时, 看不懂任何错误, 因为还没有配置, 它默认不附带任何规则
配置:
通过.eslintrc文件配置,该文件存在于项目的根目录中.
让我们添加一个typescript配置的eslint:
```json
{
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint", "prettier"],
  "extends": [
    "airbnb",
    "eslint: recommended",
    "plugin: @typescript-eslint/eslint-recommended",
    "plugin: @typescript-eslint/recommended",
    "plugin: prettier/recommended"
  ],
  "settings": {
    "import/extensions": [".js", ".jsx", ".ts", ".tsx"],
    "import/parsers": {
      "@typescript-eslint/parser": [".ts", ".tsx"]
    },
    "import/resolver": {
      "node": {
        "extensions": [".js", ".jsx", ".ts", ".tsx"]
      }
    }
  },
  "rules": {
    "semi": [2, "never"]
  }
}
```

semi: [2, nerver]
这句话可能困惑, 但这是ESLint规则的警告范围
off(or 0): 该规则被禁用
warn(or 1): 该规则是一个警告
error(or 2): 该规则抛出一个错误
第二个参数告诉ESlint, 我们想让分号永远不被使用(反之则是always). ESLint和它的插件都有很好的文档, 对于任何单个规则, 你都可以找到规则的描述和一些它什么时候通过和什么时候失败的例子
一下是作者喜欢关闭和禁用的规则:
```json
"rules": {
  "semi": [2, "never"],
  "@typescript-eslint/class-name-casing": "off",
  "@typescript-eslint/interface-name-prefix": "off",
  "@typescript-eslint/member-delimiter-style": "off",
  "@typescript-eslint/no-var-requires": "off",
  "@typescript-eslint/ban-ts-ignore": "off",
  "@typescript-eslint/no-use-before-define": "off",
  "@typescript-eslint/ban-ts-comment": "off",
  "@typescript-eslint/explicit-module-boundary-types": "off",
  "no-restricted-syntax": "off",
  "no-use-before-define": "off",
  "import/extensions": "off",
  "import/prefer-default-export": "off",
  "max-len": [
    "error",
    {
      "code": 100,
      "tabWidth": 2
    }
  ],
  "no-param-reassign": "off",
  "no-underscore-dangle": "off",
  "react/jsx-filename-extension": [
    1,
    {
      "extensions": [".tsx"]
    }
  ],
  "import/no-unresolved": "off",
  "consistent-return": "off",
  "jsx-ally/anchor-is-valid": "off",
  "sx-ally/click-events-have-key-events": "off",
  "jsx-ally/no-noninteractive-element-interactions": "off",
  "jsx-ally/no-static-element-interactions": "off",
  "react/jsx-props-no-spreading": "off",
  "jsx-ally/label-has-associated-control": "off",
  "react/jsx-one-expression-per-line": "off",
  "no-prototype-builtins": "off",
  "no-nested-ternary": "off",
  "prettier/prettier": [
    "error",
    {
      "endOfLine: "auto"
    }
  ]
}
```

git hooks
为了避免在我们的仓库中出现未提示的代码, 我们可以做的是使用Git Hooks在流程的某一点上添加ESLint. 例如, 我们可以用husky在名为pre-commit的Git hook中运行我们的linter, 在名为pre-push的Hook上运行我们的单元测试也很有用
```bash
npm install -S husky
```
package.json 添加这个节点来配置我们想在Git Hooks中运行的任务
{
  "script": {
    "lint": "eslint --ext .tsx,.ts src",
    "lint:fix": "eslint --ext .tsx, .ts --fix src',
    "test": "jest src"
  },
  "husky": {
    "hooks": {
      "pre-commit": "npm lint",
      "pre-push": "npm test"
    }
  }
}
ESLint 命令有一个特殊的选项(标志), 叫做--fix 有了这个选项, ESLint会尝试自动修复我们几乎所有的linter错误, 对这个选项要小心, 因为有时它可能会影响我们的代码风格 . 另一个有用的标志是--ext, 用来指定我们要验证的文件的扩展名, 在这里只是指.tsx和.ts文件

# 函数式编程
出了在编写JSX时遵循最佳实践, 并使用linter来执行一致性和尽早发现错误之外, 我们还可以做一件事来清理我们的代码: 遵循FP(函数式编程,Functional Programming)风格

> react遵循声明式范式,不需要告诉它如何与dom交互,你想在屏幕上看到什么, react就会为你完成工作
前面说过React是一个声明式编程, 使我们的代码更易读. 而FP则是一种声明式范式, 它避免了副作用, 数据被认为是不可变的, 使代码更容易维护和推理.
javascript 拥有一流的函数, 因为它们被当作任何其他变量对待, 这意味着你可以将一个函数作为参数传递给其他函数, 或者它可以被了一个函数返回并作为数值分配给一个变量
这使得我们可以引入高阶函数(HoFs)的概念. 高阶函数是以一个函数为参数的函数, 也可以选择一些其他参数,并返回一个函数. 返回的函数通常被增强了一些特殊行为.
让我们看一个例子.
```js
const add = (x, y) => x + y
const log = fn => (...args) => {
  return fn(...args)
}
const logAdd = log(add)
```
这个概念理解相当重要, 因为在React的世界中, 一个常见的模式是使用HOCs将我们的组件视为函数, 并以共同的行为来增强它们. 我们将在第四章“探索流行的组成模式”中看到HOCs和其他模式.

纯粹性
FP的一个重要方面是编写纯函数. 在React生态系统中, 你会经常遇到这个概念, 特别是如果你研究Redux这样的库.
纯粹性:当一个函数没有副作用时, 它就是纯粹的, 这意味着该函数不会改变任何不属于该函数本身的东西
例如, 一个改变应用程序状态的函数, 或修改在上层范围内定义的变量, 或一个触及外部实体的函数, 如文档对象模型(DOM), 被认为是不纯的. 不纯的函数更难调试, 而且大多数情况下, 不可能多次应用它们并期望得到相同的结果.
例如, 下面这个函数是纯的:
```js
const add = (x, y) => x + y
```
它可以多次运行, 总是得到相同的结果, 因为没有任何东西被存储在任何地方, 也没有任何东西被修改.
不纯的函数:
```js
let x = 0
const add = y => (x = x + y)
```
运行add(1)两次, 我们得到两个不同的结果. 第一次我们得到1, 第二次我们得到2, 即使我们用相同的参数调用同一个函数. 我们得到这种行为的原因是, 全局状态在每次执行后都被修改

在纯函数中,当我们需要改变一个变量时, 一个函数并不改变一个变量的值, 而是用一个新的值创建一个新的变量并将其返回. 这种处理数据的方式被称为不可变性(这不是类一样)
一个不可变的值==一个不能被改变的值
让我们来看一个例子
```js
const add3 = arr => arr.push(3)
const myArr = [1, 2]
add3(myArr)
add3(myArr)
```
以前居然没注意过, 变量传到props中居然传入的是变量的地址,修改这个props就会直接修改传入的值,我的天

这个函数并不遵循不变性, 因为它改变了给定数组的值. 同样,如果我们调用一个函数两次, 我们会得到不同的结果

使用concat来改变前面的函数, 使其成为不可变的
concat这个函数的实现应该是把传入的值复制给一个新的变量,这样就不会直接修改传入值的地址,所以就不会改变其传入的值
```js
const add3 = arr => arr.concat(3)
const myArr = [1, 2]
const result1 = add3(myArr)
const result3 = add3(myArr)
我们运行完两次函数后, myArr仍然是它的原始值,牛的
```

# 函数柯里化(Currying)
FP中一个常见的技术是Currying. Currying是指将一个接受多个参数的函数转换为一次一个参数的函数并返回了一个函数的过程. 
把之前的add函数转化为一个柯里化函数
```js
const add = (x, y) => x + y
```
将这个函数转化为以下:
```js
const add = x => y => x + y
// const add = (x) => {
//   return (y) => {
//     return x + y
//   }
// }
const add1 = add(1)
add1(2)
add1(3)
```
FP中一个重要概念是组合. 函数(和组件) 可以被组合, 以产生具有更高级功能和属性的新函数
这是一种相当方便的编写函数的方式, 因为, 由于第一个值是应用第一个参数后存储的, 我们可以多次重复使用第二个函数
```js
const add = (x, y) => x + y
const square = x => x * x
const addAndSquare = (x, y) => square(add(x, y))
```
Fp and UIs
```js
UI = f(state)
```
组件可以组成最终的UI,这也是FP的一个属性
我们用React构建UI的方式和FP的原则有很多相似之处, 我们对它的认识越深, 我们的代码就越好

> yarn 和 npm现在不用代理都很快

