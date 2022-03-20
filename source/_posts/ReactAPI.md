---
title: ReactAPI
date: 2022-03-09 10:20:51
tags: [react]
categories: [react]
---

# 无障碍
每一个HTML表单控件, 如input和textarea都需要有无障碍标签. 我们需要提供描述性标签, 这些标签也会暴露给屏幕阅读器
例:
img标签的alt属性,a标签的内容被视为描述
input标签使用id和label的for属性共享相同的值, 这会在两个元素之间创建关联, 指示浏览器将标签的内容公开为复选框的可访问名称
```html
<input type="checkbox" id="tequila">
<label for="tequila">Chamukos tequila</label>
```
使用ARIA可以更改元素的可访问名称
aria-label
```html
<button aria-label="Add Chamukos tequila to cart">dd to cart</button>
```
aria-labelledby
该aria-labelledby属性将id ref作为其值
```html
<input type="serarch" aria-labelledby="this">
<button id="this">Search</button>
```
> for属性在jsx中被写成htmlFor

# 代码拆分
1. import() 这是createReactApp已经配置好的webpack所直接提供的,不需要额外的配置即可食用. Nextjs中同样如此
```js
import("./math").then(math => {
  console.log(math.add(16, 26))
})
```
在使用babel时,需要确保babel能够解析动态导入语法, 但不对其进行转换. 为此, 你将需要
@babel/plugin-syntax-dynamic-import

2. React.lazy
> React.lazy和Suspense还不能用于服务器渲染.
如果你想在服务区渲染应用中进行代码拆分, 推荐Loadable Components

React.lazy函数可以让你把动态导入作为一个普通组件来渲染
```js
const OtherComponent = React.lazy(() => import('./OtherComponent'))
```
这将在该组件首次渲染时自动加载包含OtherComponent的包.
React.lazy需要一个必须调用动态import()的函数. 这必须返回一个Promise, 该Promise解析到一个包含React组件的默认导出模块
然后, lazy组件应该在一个Suspense组件内呈现, 这允许我们在等待lazy组件加载时显示一些(fallback)后备内容(如加载指示器)
```js
import React, {Suspense} from 'react
const OtherComponent = React.lazy(() => import('./OtherComponent'))
const AnotherComponent = React.lazy(() => import('./AnotherComponent'))
function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherCompoent>
          <AnotherComponent>
        </section>
      </Suspense>
    </div>
  )
}
```

Introducing Error Boundaries(错误边界)
在用户界面的一部分出现javascript错误,不应该破坏整个应用程序. 为了解决react用户这个问题, react16引入了“error boundary“这个新概念
错误边界在渲染过程中, 在生命周期方法中,以及在它们下面的整个树的构造函数中捕捉错误
note:
错误边界不捕捉以下错误
Event handlers(事件处理程序)
异步代码(Asynchronous code)例如setTimeout或requestAnimationFrame回调
服务器端端渲染(Server side rendering)
错误边界本身抛出的错误(而不是其子代)

错误边界:
如果一个类组件定义了 static getDerivedStateFromError() 或 componentDidCatch()这两个生命周期方法中的任何一个,它就会成为一个错误边界. 使用 static getDerivedStateFromError()在错误被抛出后渲染一个后退的用户界面. 使用componentDidCatch()来记录错误信息
```js
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props)
    this.state = { hasError: false }
  }
  static getDerivedStateFromError(error) {
    // 更新state, 以便下次渲染时显示回退的用户界面
    return { hasError: true }
  }
  componentDidCatch(error, errorInfo) {
    // 你也可以将错误记录在一个错误报告服务上
    logErrorToMyService(error, errorInfo)
  }
  render() {
    if(this.state.hasError){
      // 你可以呈现任何后备ui
      return <h1>Something went wrong</h1>
    }
    return this.props.children
  }
}
```
然后可以用其做常规组件
```js
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```
错误边界的工作方式类似于javascript的catch{}块, 但对于组件而言. 只有类组件可以成为错误边界. 在实践中, 大多数情况下 你想声明一次错误边界组件并在整个应用程序中使用它.

基于路由的代码拆分(Route-based)
```js
import React, {Suspense, lazy} from 'react'
import {BrowserRouter as Router, Routes, Route} from 'react-router-dom'

const Home = lazy(() => import('./routes/home'))
const About = lazy(() => import('./routes/About'))
const App = () => {
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path='/' element={<Home />}>
        <Route path='/about' element={<About />}>
      </Routes>
    <Suspense>
  </Router>
}
```
React.lazy只支持默认导出,如果一个组件使用命名导出,则可以用一个中间组件改为默认导出
```js
// ManComponents.js
export const MyComponent = /* ... */
export const MyUnusedComponent = /* ... */
```
```js
// MyComponent.js
export {MyComponent as default} from './ManyComponents.js'
```
```js
import React, {lazy} from 'react'
const MyComponent = lazy(() => import("./MyComponent.js"))
```
# Context
以前的理解是跨组件传递props, 不用一级一级向下传递
```js
const ThemeContext = React.createContext('light')
class App extends React.Component {
// 使用Provider将当前主题传递给下面的树
// 任何组件都可以读取它, 不管它有多深
// 这个例子中, 我们把dark作为当前值传递
  render() {
    return (
      <ThemeContext.Provider value='dark'>
        <Toolbar />
      </ThemeContext.Provider>
    )
  }
}

// 中间的组件不需要明确的将主题传递下去
function Toolbar() {
  return (
    <div>
      <ThemedButton />
    </div>
  )
}
class ThemedButton extends React.Component {
  // 指定一个contextType来读取当前猪蹄上下文
  // react会找到上面最接近的主题提供者并使用其值
  // 在这个例子中, 当前主题是dark
  static contextType = ThemeContext
  render() {
    return <Button theme={this.context} />
  }
}
```
NOTE:
上下文主要用于某些数据需要被不同嵌套级别的许多组件所访问的时候. 少用它, 因为它会导致组件的重用更加困难
如果你只想避免将一些道具传递到许多层, 那么组件组合通常是比上下文更简单的解决方案

就是把组件提到根组件的位置,然后把该组件当参数向下传递,这样写有些混乱?
## API
### `const MyContext = React.createContext(defaultValue)`
创建一个Context对象. 当React渲染一个订阅了这个Context对象的组件时, 它将从树上最接近的匹配提供者哪里读取当前上下文值

defaultValue参数只在一个组件在树中没有匹配的Provider时使用. 这个默认值有助于在不包裹组件的情况下单独测试组件. Note: 传递为定义的Provider值不会导致消耗组件使用defaultValut

### `<MyContext.Provider value={/* some value */}></MyContext.Provider>`
每个Context对象都有一个Provider react组件, 允许消费组件订阅上下文的变化
(Provider)提供者组件接受一个值道具, 将其传递给作为该提供者后代的消费组件. 一个提供者可以连接到许多消费者. 提供者可以被嵌套以覆盖树中更深的值
每当提供者的值props发生变化时, 作为提供者后裔的所有消费者将重新渲染. 从提供者到其后代消费者(包括 .contextType and useContext) 的传播不受shouldComponentUpdate方法的影响, 因此即使祖先组件跳过更新, 消费者也会被更新
变化时通过使用Object.is相同的算法比较新的和旧的值来确定的

Note: 使用对象时如果直接传递对象,就会导致消费组件每次都会被渲染(以前学过,那个好像是匿名函数还是啥来着),解决方法就是创建一个对象值来代替(应该,或者必须使用useState之类的?)
```js
class App extends React.Component {
  render () {
    // 这样会导致每次都渲染使用它的消费者组件
    return (
      <MyContext.Provider value={{something: 'something'}}>
        <Toolbar>
      </MyContext.Provider>
    )
  }
}
```
解决方法:
```js
class App extends React.Component {
  constructor(props) {
    super(props)
    this.state={
      value: {something: 'someting'}
    }
  }
  render () {
    return (
      <MyContext.Provider value={this.state.value}>
        <Toolbar>
      </MyContext.Provider>
    )
  }
}

```
### Class.contextType
类上的contextType属性可以分配给由React.createContext()创建的Context对象. 使用这个属性可以让你使用this.context使用该context类型的最近当前值. 你可以在任何生命周期的方法中引用这个, 包括render函数
```js
class MyClass extends React.Component {
  componentDidMount() {
    let value = this.context
  }
  componentDidUpdate() {
    let value = this.context
  }
  componentWillUnmount() {
    let value = this.context
  }
  render() {
    let value = this.context
  }
}
MyClass.contextType = MyContext
```
使用多个上下文
```js
const ThemeContext = React.createContext('light')
const UserContext = React.createContext({
  name: 'Guest'
})
class App extends React.Component {
  render () {
    const {signedInUser, theme} = this.props
    return (
      <ThemeContext.Provider value={theme}>
        <UserContext.Provider value={signedInUser}>
          <Lazyout />
        </UserContext.Provider>
      </ThemeContext.Provider>
    )
  }
}
function Lazyout() {
  return (
    <div>
      <Sidebar/>
      <Content/>
    </div>
  )
}
function Content() {
  // 为了保持上下文重新渲染速度, React需要让每个上下文消费者(Consumer)成为树中的一个独立节点
  return (
    <ThemeContext.Consumer>
      {
        theme => (
          <UserContext.Consumer>
            {
              user => (
                <ProfilePage user={user} theme={theme}>
              )
            }
          </UserContext.Consumer>
        )
      }
    </ThemeContext.Consumer>
  )
}
```
# 转发参考(Forwarding Refs)
参考转发是一种技术, 它可以通过一个组件自动将一个参考信息传递给它的一个子组件. 对于应用程序中的大多数组件来说, 这通常是没有必要的. 然后, 它对某些种类的组件是有用的, 特别是在可重用组件库中.
通俗讲ref就是把input元素选中,供其他元素直接操作dom,比如点击按钮获取焦点foces()
基础用法:
```js
// 这段代码呢,很好理解,
// 创建ref
// 将其赋值给input标签
// react渲染完元素后,通过useEffect方法调用current上的focus方法聚焦到input标签上
function App () {
  const ref = React.createRef()
  useEffect(() => {
    ref.current.focus()
  },[])
  return <input ref={ref}>
}
```
如果是传递给组件ref的话就不行,必须使用React.forwardRef方法传递
```js
const Button = React.forwardRef((props, ref) => {
  return (
  <button ref={ref} {...props}>
    {props.children}
  </button>
  )
})
function App () {
  const ref = React.createRef()
  useEffect(() => {
    ref.current.focus()
  }, [])
  return <Button ref={ref} className='helo'>Click me</Button>
}
```
class 组件里面传递
如果将forwardRef与类组件一起使用, 我们不能仅仅用forwardRef包装我们的类组件. 但是, 我们可以在提供给forwardRef的渲染函数中使用类组件:
```js
import React from 'react'
class Input extends React.Component {
  componentDidMount() {
    // 测试是否有
    console.log(this.props)
  }
  render() {
    const { innerRef, ...props } = this.props
    return (
      <div style={{background: "bule"}}>
        <input ref={innerRef} {...props}/>
      </div>
    )
  }
}
const InputForwardingRef = React.forwardRef((props, ref) => {
  return <Input {...props} innerRef={ref} />
})
export default InputForwardingRef
```
不能使用ref prop, 因为这只有在组件使用forwardRef时才有效

不懂这里为什么不能使用ref,而必须用一个innerRef代替?
好像是ref不能传递直接传递给类组件

## 离谱,不直接使用ref,而改名后把ref传递进去也是可以啊,为什么偏偏要使用forwardRef
```js
const InnerRe = (props) => {
  console.log('props',props)
  return <input ref={props.innerRef} />
}
function App () {
  const ref = React.createRef()
  return (
    <InnerRe innerRef={ref}>
  )
}
```
# API
React.memo 为高阶组件
如果你的组件在相同props的情况下渲染相同的结果, 那么你可以通过将其包装在React.memo中调用, 以此通过记忆组件渲染结果的方式来提高组件的性能表现. 
React.memo 仅检查props变更, 如果函数组件被memo包裹,且其实现中有useState, useReducer或useCOntext的hook, 当state或context发生变化时, 它仍会重新渲染

他还有第二参数,默认情况下其只会对复杂对象做浅层对比, 如果你想要控制对比过程, 那么请将中调用的比较函数通过第二参数传入来实现
```js
function MyComponent(props) {

}
function areEqual(prevProps, nextProps) {
  // 与shouldComponentUpdate() 方法不同的是, 如果props相等, areEqual则返回true, 反之则相反
}
export default React.memo(MyComponent, areEqual)
```
此方法仅作为性能优化的方式存在. 不要依赖它来阻止渲染,因为它会产生bug

## render()
render被调用时, 它会检查this.props和this.state的变化并返回以下类型:
react元素
数组或fragments
Portals
字符串或数值类型
布尔类或null

render()函数应该为纯函数, 着意味着在不修改state的情况下, 每次调用时都返回相同的结果, 并且它不会直接与浏览器交互
如需与浏览器进行交互, 请在componentDidMount()或其他生命周期方法中执行你的操作. 保持render() 为纯函数, 可以使组件更容易思考
Note: shouldComponentUpdate() 返回false, 则不会调用render()

## constructor
如果不初始化state或不进行方法绑定, 则不需要为react组件实现构造函数
在为React.Component子类实现构造函数时, 应在其他语句之前调用super(props) 否则, this.props在构造函数中可能会出现未定义的bug

只能在构造函数中为this.state赋值. 如需在其他地方赋值, 你应使用this.setState()代替
要避免在构造函数中引用任何副作用或订阅. 如遇到此场景, 请将对应的操作放置在componentDidMount中

Note:
避免将props值赋值给state 如此做毫无必要(你可以直接使用this.props.color)
如果把props赋值给state还会产生bug, 更新props中的color时, 并不会影响state
只有你在可以忽略props更新的情况下使用. 此时, 应将props重命名为initialColor或defaultColor. 必要时,你可以修改它的key, 以强制重置其内部state

## componentDidMount()
会在组件挂在后(插入DOM树)立即调用.依赖于Dom节点的初始化应该放在这里. 如需通过网络请求获得数据, 此处是实例化请求的好地方

这个方法是比较适合添加订阅的地方. 如果添加了订阅, 请不要忘记在componentWillUnmount() 里取消订阅

你可以在componentDidMount()里调用setState(). 它将触发额外渲染, 但此渲染会发生在浏览器更新屏幕之前. 如此保证了即使在render()两次调用的情况下, 用户也不会看到中间状态. 请谨慎使用该模式, 因为它会导致性能问题. 通常, 你应该在constructor()中初始化state. 如果你的渲染依赖于DOM节点的大小或位置, 比如实现modals和tooltips等情况下, 你可以使用此方法处理

## componentDidUpdate()
componentDidUpdate(prevProps, prevState, snapshot)
会在更新后立即被调用. 首次渲染不会执行此方法
当组件更新后, 可以在此对DOM进行操作. 如果你对更新前后对props进行比较, 也可以选择在此进行网络请求. (例如当props未发生变化时, 则不会执行网络请求)
```js
componentDIdUpdate(prevProps) {
  if (this.props.userID !== prevProps.userID){
    this.fetchData(this.props.userID)
  }
}
```
可以在componentDidUpdate中直接调用setState(), 但请注意它必须被包裹在一个条件语句里, 如上述例子中的那样, 否则会导致死循环. 它还会导致额外的重新渲染,虽然用户不可见,但会影响组件性能. 不要将props ”镜像“ 给state, 请考虑直接使用props,不然会产生bug

如果组件实现了getSnapshotBeforeUpdate()生命周期, 则它的返回值将作为componentDidUpdate的第三个参数‘snapshot’参数传递. 否则此参数将为undefind

Note:
如果shouldComponentUpdate()返回值为false, 则不会调用componentDidUpdate()

## componentWillUnmount()
会在组件卸载及销毁之前直接调用. 在此方法中执行必要清理操作, 例如: 清除timer, 取消网路请求或清楚在componentDidMout中创建的订阅
不应该调用setState(), 因为该组件将永远不会重新渲染.
组件实例卸载后, 将永远不会再挂载它

## shouldComponetUpdate(nextProps, nextState)
根据shouldComponetUpdate()的返回值, 判断React组件的输出是否受当前state或props更改的影响
当props或state发生变化时, shouldComponentUpdate会在渲染执行之前被调用. 返回值默认为true. 首次渲染或使用`强制渲染`forceUpdate()时不会调用该方法

此方法仅作为性能优化方式存在, 不要企图依赖此方法来阻止渲染,这会产生bug. 应该考虑内置的PureCoponent组件, 而不是手动编写此生命周期. PureComponet会对props和state进行浅层比较, 并减少了跳过必要更新的可能性

如果一定要手动编写此函数, 可以将this.props与nextProps以及this.state于nextState进行比较, 并返回false以告知React可以跳过更新. 请注意, 返回false并不会阻止子组件在state更新时重新渲染

不建议在此生命周期中进行浅层比较或使用JSON.stringify(). 这样的非常影响效率, 且会损害性能

目前, 如果此生命周期返回false, 则不会调用UNSAFE_componentWillUpdate(), render() 和 componentDidUpdate(). 后续的版本, react可能会将此生命周期视为提示而不是严格的指令, 并且, 当返回false时,仍可能导致组件重新渲染

## static getDerivedStateFromProps(props, state)
此生命周期会在调用render方法之前调用, 并且在初始化以及后续更新都会被调用. 它应返回一个对象来更新state, 如果返回null则不更新任何内容

 此方法适用于罕见的用例, 即state的值在任何时候都取决于props. 例如实现<Transition>组件可能很方便, 该组件会比较当前组件与下一个组件, 以绝对针对那些组件进行转场动画.

 派生状态会导致代码冗余, 并使组件难以维护. 

如果你需要执行副作用(例如, 数据提取或动画)以响应props中的更改, 请改用componentDIdUpdate
如果你只想在prop更时重新计算某些数据, 清使用memoization helper代替
如果你想在prop更新时“重制”某些state, 请考虑使组件完全受控或使用key使组件完全不受控代替

此方法无法访问组件实例. 如果你需要, 可以通过提取组件props的纯函数及class之外的状态, 在getDerivedStateFromProps()和其他class方法之间重用代码
Note: 不管原因是什么, 都会在每次渲染前渲染此方法. 这与UNSAFE_componentWillReceiveProps形成对比,后者仅在父组件重新渲染时触发, 而不是内部调用setState时

## getSnapshotBeforeUpdate(prevProps, prevState)
此生命周期在最近一次渲染输出(提交到DOM节点)之前调用. 它使得组件能在发生更改之前从DOM中捕获一些信息(例如,滚动位置). 此生命周期方法的任何值将作为参数传递给componentDidUpdate()
此用法并不常见, 但它可能出现在UI处理中, 如需要以特殊方式处理滚动位置的聊天线程等
应返回snapshot的值(或null)
例如:
```js
class ScrollingList extends React.Componet {
  constructor(props) {
    super(props)
    this.listRef = React.createRef()
  }
  getSnapshotBeforeUpdate(prevProps, prevState) {
    // 我们是否在list中添加新的items
    // 捕获滚动位置以便我们稍后调整滚动位置
    if (prevProps.list.length < this.props.list.length) {
      const list = this.listRef.current
      return list.scrollHeight - list.scrollTop
    }
    return null
  }
  componentDidUpdate(prevProps, prevState, snapshot) {
    // 如果我们 snapshot有值, 说明我们刚刚添加了新的items
    // 调整滚动位置使得这些新的items 不会将旧的items推出试图.
    if (snapshot !== null) {
      const list = this.listRef.current
      list.scrollTop = list.scrollHeight - snapshot
    }
  }
  render () {
    return (
      <div ref={this.listRef}>
        {/* ...contents... */}
      </div>
    )
  }
}
```

## static getDerivedStateFromError()
static getDerivedStateFromError(error)

此生命周期会在后代组件抛出错误后被调用. 它将抛出的错误作为参数, 并返回一个值以更新state

```js
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props)
    this.state = { hasError: false }
  }
  static getDerivedStateFromError(error) {
    return { hasError: true }
  }
  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>
    }
    return this.props.children
  }
}
```
Note: 此生命周期会在渲染阶段调用, 因此不允许出现副作用. 如遇此类情况, 请改用componentDidCatch()

## componentDidCatch(error, ifno)
此生命周期在后代组件抛出错误后被调用. 它接收两个参数

1. error 抛出的错误
2. info 带有componentStack key的对象, 其中包含`有关组件引发错误的栈信息

componentDidCatch() 会在“提交(commit)”阶段被调用, 因此允许执行副作用. 它应该用于记录错误之类的情况:
```js
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props)
    this.state = { hasError: false }
  }
  static getDerivedStateFromError(error) {
    return { hasError: true }
  }
  componentDidCatch(error, info) {
    // “组件堆栈” 例子:
    // in ComponentThatThrows (created by App)
    // in ErrorBoundary (create by App)
    // in div (created by App)
    // in App
    logComponentStackToMyService(info.componentStack)
  }
  render() {
    if (this.state.hasError) {
      // 你可以渲染任何自定义的降级 UI
      return <h1>Something went wrong.</h1>
    }
    return this.props.children
  }
}
```
React 的开发和生产构建版本在componentDidCatch()的方式上有轻微的差别. 
在开发者模式下, 错误会冒泡至window, 这意味着window.onerror或window.addEventListener('error', callback) 会中断这些已经被componentDidCatch()捕获的错误
相反, 在生产模式下, 错误不会冒泡, 这意味着任何根错误处理器只能接受那些没有被显式地componentDidCatch()捕获的错误

Note: 如果发生错误, 你可以通过调用setState使用componentDidCatch()渲染降级UI, 但在未来的版本中将不推荐这样做. 可以使用静态getDerivedStateFromError()来处理降级渲染

## 过时的声明周期以UNSAFE开头

## 其他API
生命周期方法是react主动调用, 这些api则需要自己在组件中调用

只有两个方法: setState() 和 forceUpdate()

### setState
`setState(updater, [callback])`
setState()将对组件state的更改排入队列, 并通知React需要使用更新后的state重新渲染此组件及其子组件. 这是用于更新用户界面以响应事件处理器和处理服务区数据的主要方式
将setState() 视为请求而不是立即更新组件的命令. 为了更好的感知性能, React会延迟调用它, 然后通过一次传递更新多个组件. react并不会保证state的变更会立即生效

setState() 并不会立即更新组件. 它会批量推迟更新. 这使得在调用setState后立即读取this.state成为了隐患. 为了消除隐患, 请使用componentDidUpdate或setState的回调函数(setState(updater, callback)), 这两种方式都可以保证在应用更新后触发. 如需基于之前state来设置当前state, 请阅读下述关于参数updater的内容

除非shouldComponentUpdate返回false, 否则setState()将始终执行重新渲染操作. 如果可变对象被使用, 则无法在shouldComponetUpdate中实现条件渲染, 那么仅在新旧状态不一时调用setState()可以避免不必要的重新渲染

参数一为带有形式参数的updater函数
(state, props) => stateChange

updater函数中接受的state和props都保证为最新. updater的返回值与state进行浅合并.

setState()的第二个参数为可选的回调函数, 它将在setState完成合并并重新渲染组件后执行. 通常, 我们建议使用componentDidUpdate来代替此方法

setState()的第一个参数除了接受函数外, 还可以接受对象类型

`setState(stateChange[, callback])`
stateChange会将传入的对象浅层合并到新的state中, 例如, 调整购物车商品数
`this.setState({quantity: 2})`
这种形式的setState()也是异步的, 并且在同一周期内会对多个setState进行批处理. 例如, 如果在同一周期内多次设置商品数量增加, 则相当于
```js
Object.assign(
  previousState,
  {quantity: state.quantity + 1},
  {quantity: state.quantity + 1}
)
```
后调用的setState()将覆盖同一周期内先调用setState的值, 因此商品数仅增加一次. 如果后续状态取决于当前状态, 我们建议使用updater函数的形式代替
```js
this.setState((state) => {
  return {quantity: state.quantity + 1}
})
```

### forceUpdate()
component.forceUpdate(callback)
默认情况下, 当组件的state或props发生变化时, 组件将重新渲染. 如果render() 方法依赖于其他数据, 则可以调用forceUpdate()强制让组件重新渲染

调用forceUpdate() 将致使组件调用render()方法, 此操作会跳过该组件的shouldComponentUpdate(). 但其子组件会触发正常的生命周期方法, 包括shouldComponentUpdate()方法. 如果标记发生变化, React仍将只更新DOM
通常你应该避免和使用forceUpdate(), 尽量在render()中使用this.props和this.state

### Class属性
#### defaultProps

defaultProps可以为Class组件添加默认props. 这一般用于props未赋值, 但又不能为null的情况. 例如
```js
class CustomButton extends React.Component {

}
CustomButton.defaultProps = {
  color: 'bule'
}
```
如果未提供props.color, 则默认为‘bule'
```js
render() {
  return <CustomButton /> // 默认为bule
}
```
如果props.color被设置为null,则它将保持为null
```js
render() {
  return <CustomButton color={null}/> 
}
```

#### displayName
displayName字符串多用于调试消息. 通常,你不需要设置它, 因为它可以根据函数组件或class组件的名称推断出来. 如果调试时需要显示不同的名称或创建高阶组件, 请参阅`使用displayname轻松进行调试`

### 实例属性
#### props
this.props 包括被该组件调用者定义的props. `组件 & Props`
Note: this.props.children 是一个特殊的prop, 通常由JSX表达式中的子组件组成, 而非组件本身的定义

#### state
组件中的state包含了随时可能发生数据变化的数据. state由用户自定义, 它是一个普通javascript对象
如果某些值未用于渲染或数据流(例如, 计数器ID), 则不必将其设置为state. 此类值可以在组件实例上定义

永远不要直接改变this.state, 因为后续调用的setState()可能会替换掉你的改变. 请把this.state看作是不可变的.

## ReactDOM

### render()
```js
ReactDOM.render(element, container[, callback])
```
在提供的container里渲染一个React元素, 并返回对该组件的引用(或者针对无状态组件返回null)

### hydrate()
与render() 相同, 但它用于服务端渲染的容器中对HTML的内容进行hydrate操作. React会尝试在已有标记上绑定事件监听器
React希望服务端与客户端渲染的内容完全一致. React可以弥补文本内容的差异, 但是你需要在不匹配的地方进行修复.
开发者模式下回对hydration操作过程中的不匹配进行警告. 但并不能保证在不匹配的情况下, 修补属性的差异. 由于性能关系, 这一点非常重要, 因为大多是应用中不匹配的情况很少见, 并且验证所有标记的成本非常昂贵
如果单个元素的属性或者文本内容, 在服务端和客户端之间无法避免差异(比如: 时间戳), 则可以为元素suppressHydrationWarnning={true}来消除警告. 这种方式只在一级深度上有效, 应只作为一种应急方案(escape hatch). 请不要过度使用! 除非他只是内容文本, 否则React仍不能尝试修补差异, 因此未来更新之前, 仍会保持不一致

如果你执意要在服务端与客户端渲染不同的内容, 你可以采用双重渲染(two-pass). 在客户端渲染不同内容的组件可以读取类似于this.state.isClient的state变量, 你可以在componentDidMount()里将它设置为true. 这种方式在初始渲染过程中会与服务端渲染相同的内容, 从而避免不匹配的情况出现, 但hydration操作之后, 会同步进行额外的渲染操作. 注意, 因为进行了两次渲染, 这种方式会使组件渲染变慢, 请小心使用

### unmountComponentAtNote()
```js
ReactDOM.unmountComponentAtNode(container)
```
从DOM中卸载组件, 会将其事件处理器(event handlers)和state一并清楚. 如果指定容器上没有对应已挂载的组件, 这个函数什么也不会做. 如果组件被移除会返回true, 如果没有组件可被移除将返回false

### findDOMNode()
```js
ReactDOM.findDOMNode(component)
```
Note: findDOMNode 是一个访问底层DOM节点的应急方案(escape hatch). 在大多数情况下, 不推荐使用该方法, 因为它会破坏组件的抽象结构. 严格模式下该方法已弃用

如果组件已经被挂载到DOM上, 此方法会返回浏览器中相应的原生DOM元素. 此方法对于从DOM中读取值很有用, 例如获取表单字段的值或者执行DOM检测(performing DOM measurements). 大多数情况下, 你可以绑定一个ref到DOM节点上,完全可以避免使用findDOMNode

当组件渲染的内容为null或false时, findDOMNode 也会返回null. 当组件渲染的是字符串时, findDOMNode 返回的是字符串对应的DOM节点. 从React16开始, 组件可能会返回有多个字节的的fragment, 在这种情况下, findDOMNode会返回第一个非空自节点对应的DOM节点

Note: findDOMNode 只在已挂载的组件上可用(即, 已经放置在DOM中的组件). 如果你尝试调用未挂载的组件(例如在一个还未创建的组件上调用render()中的findDOMNode())将会引发错误
findDOMNode 不能用于函数组件

### createPortal()
```js
ReactDOM.createPortal(child, container)
```
创建portal. Portal将提供一种将子节点渲染到DOM节点中的方式, 该节点存在于DOM组件的层次结构之外 

## ReactDOMServer
ReactDOMServer对象允许你将组件渲染成静态标记. 通常, 它被使用在Node服务端上

### renderToString()
```js
ReactDOMServer.renderToString(element)
```
将React元素渲染为初始HTML. React将返回一个HTML字符串. 你可以使用此方法在服务端生成HTML, 并在首次请求时将标记下发, 以快速页面加载速度, 并允许搜索引擎爬取你的页面已达到SEO优化目的
如果你在已有服务端渲染标记的节点上调用ReactDOM.hydrate()方法, React将会保留该节点并且只进行事件处理绑定, 从而让你有一个非常高性能的首次加载体验

### renderToStaticMarkup()
```js
ReactDOMServer.renderToStateicMarkup(element)
```
此方法与renderToString相似, 但此方法不会在React内部创建额外的DOM属性, 例如data-reactroot. 如果你希望把React当作静态页面生成器来使用, 此方法会非常有用, 因为去除额外的属性可以节省一些字节

如果你计划在前段使用React以使标记可交互, 请不要使用此方法. 你可以在服务端上使用renderToString或在前段上使用ReactDOM.hydrate()来代替

### renderToNodeStream()
ReactDOMServer.renderToNodeStream(element)

将一个React元素渲染成其初始HTML. 返回一个可输出HTML字符串的可读流. 通过可读流输出HTML完全等同于ReactDOMServer.renderToString返回的HTML. 你可以使用本方法在服务器上生成HTML, 并在初始请求时将标记下发, 以加快页面加载速度, 并允许搜索引擎抓取你页面已达到SEO优化的目的.

### renderToStaticNodeStream(element)
和renderToNodeStream()类似, 参考renderTostaticMarkup()

## DOM元素
在React中, 所有的DOM特性和属性(包括事件处理)都应该是小驼峰命名方式. 例如, 与HTML中的tabindex属性对应的React属性是tabIndex. 例外的情况时aria-*以及`data-*`,一律使用小写字母命名. 比如,你仍然可以用aria-label作为aria-label

React与HTML之间的很多属性存在差异:
### checked
input组件的type类型为checkbox或radio时,组件支持checked属性. 你可以使用它来设置组件是否被选中. 这对于构建受控组件很有帮助, 而defaultChecked则时非受控组件属性, 用于设置组件首次挂载是否被选中

### className
className属性用于指定css的class
如果你在React中使用Web Components(这是一种不常见的使用方式), 请使用class属性代替

### dangerouslySetInnerHTML
是React为浏览器DOM提供innerHTML的替代方案. 通常来讲, 使用代码设置HTML存在风险, 因为很容易无意中使用户暴露于`跨站脚本(XSS)`的攻击. 因此, 你可以直接在React设置HTML, 但当你想设置dangerouslySetInnerHTML时, 需要向其传递包括key为__html的对象, 以此来警示你. 例如:
```js
function createMarup(){
  return {__html: 'First & middot; Second'}
}
function MyComponent() {
  return <div dangerouslySetInnerHTML={createMarup()}/>
}
```

### htmlFor
由于for在javaScript中是保留字, 所以React元素使用了htmlFor来代替

### onChange
onChange事件与预期行为一致: 每当表单字段变化时, 该事件都会被触发. 我们故意没有使用浏览器已有的默认行为, 是因为onChange在浏览器中的行为和名称不应对, 并且React依靠了该事件实时处理用户输入

### selected
如果要将option标记为已选中状态, 请在select的value中引用该选项. 

### style
Note: 在文档中, 不问例子为了方便,直接使用了style, 但是通常不推荐将style属性作为设置元素样式的主要方式. 在多数情况下, 应使用className属性来引用外部CSS样式表中定义的class, style在React应用中多用于在渲染过程中添加动态计算的样式.
style接受一个采用小驼峰命名属性的javascript对象, 而不是css字符串. 这与DOM中style的javascript属性是一致的, 同时会更高效, 且能预防跨站脚本(XSS)的安全漏洞. 例如
```js
const divStyle = {
  color: 'bule',
  backgroundImage: 'url(' + imgUrl + ')',
}
function HelloWorldComponent(){
  return <div style={divStyle}>Hello World! </div>
}
```
Note: 样式不会自动添加补齐前缀. 如需支持旧版浏览器, 手动添加
```js
const divStyle = {
  WebkitTransition: 'all',
  msTransition: 'all'
}
```
React会自动添加“px”后缀到内联样式为数字的属性后

### suppressContentEditableWarning
通常, 当拥有子节点的元素被标记为contentEditable时, React会发出一个警告, 因为这不会生效. 该属性将禁止此警告. 尽量不要使用该属性, 除非你要构建一个类似Draft.js管理contentEditable属性的库

### suppressHydrationWarning

如果你使用React服务端渲染, 通常会在当服务器与客户端渲染不同的内容时发出警告. 但是, 在一些极少数情况下, 很难甚至不可能保证内容的一致性. 例如, 在服务端和客户端上, 时间戳通常是不同的.
如果设置次属性为true, React将不会警告你属性与元素内容不一致. 它只会对元素一级深度有效, 并且打算作为应急方案. 因此不要过度使用它

### value
<input> <select> 和 <textarea> 组件支持value属性. 你可以使用它为组件设置value值, 这对于构建受控组件是非常有帮助的. defaultValue属性对应对是非受控组件属性, 用于设置组件第一次挂载时的value

### ALL Supported html Attributes
在react16中,任何标准的或自定义的DOm属性都是完全支持的
React为DOM提供了一套以JavaScript为中心的API. 由于React组件经常采用自定义货和DOM相关的props的关系, React采用了小驼峰命名的方式, 正如DOM APIs那样
```js
<div tabIndex={-1} /> // node.tabIndex
<div className="Button" /> // node.className
<div readOnly={true} /> // node.readOnly
```
React支持的DOM属性有:
```
accept acceptCharset accessKey action allowFullScreen alt async autoComplete
autoFocus autoPlay capture cellPadding cellSpacing challenge charSet checked
cite classID className colSpan cols content contentEditable contextMenu controls
controlsList coords crossOrigin data dateTime default defer dir disabled
download draggable encType form formAction formEncType formMethod formNoValidate
formTarget frameBorder headers height hidden high href hrefLang htmlFor
httpEquiv icon id inputMode integrity is keyParams keyType kind label lang list
loop low manifest marginHeight marginWidth max maxLength media mediaGroup method
min minLength multiple muted name noValidate nonce open optimum pattern
placeholder poster preload profile radioGroup readOnly rel required reversed
role rowSpan rows sandbox scope scoped scrolling seamless selected shape size
sizes span spellCheck src srcDoc srcLang srcSet start step style summary
tabIndex target title type useMap value width wmode wrap
```
SVG属性
```
accentHeight accumulate additive alignmentBaseline allowReorder alphabetic
amplitude arabicForm ascent attributeName attributeType autoReverse azimuth
baseFrequency baseProfile baselineShift bbox begin bias by calcMode capHeight
clip clipPath clipPathUnits clipRule colorInterpolation
colorInterpolationFilters colorProfile colorRendering contentScriptType
contentStyleType cursor cx cy d decelerate descent diffuseConstant direction
display divisor dominantBaseline dur dx dy edgeMode elevation enableBackground
end exponent externalResourcesRequired fill fillOpacity fillRule filter
filterRes filterUnits floodColor floodOpacity focusable fontFamily fontSize
fontSizeAdjust fontStretch fontStyle fontVariant fontWeight format from fx fy
g1 g2 glyphName glyphOrientationHorizontal glyphOrientationVertical glyphRef
gradientTransform gradientUnits hanging horizAdvX horizOriginX ideographic
imageRendering in in2 intercept k k1 k2 k3 k4 kernelMatrix kernelUnitLength
kerning keyPoints keySplines keyTimes lengthAdjust letterSpacing lightingColor
limitingConeAngle local markerEnd markerHeight markerMid markerStart
markerUnits markerWidth mask maskContentUnits maskUnits mathematical mode
numOctaves offset opacity operator order orient orientation origin overflow
overlinePosition overlineThickness paintOrder panose1 pathLength
patternContentUnits patternTransform patternUnits pointerEvents points
pointsAtX pointsAtY pointsAtZ preserveAlpha preserveAspectRatio primitiveUnits
r radius refX refY renderingIntent repeatCount repeatDur requiredExtensions
requiredFeatures restart result rotate rx ry scale seed shapeRendering slope
spacing specularConstant specularExponent speed spreadMethod startOffset
stdDeviation stemh stemv stitchTiles stopColor stopOpacity
strikethroughPosition strikethroughThickness string stroke strokeDasharray
strokeDashoffset strokeLinecap strokeLinejoin strokeMiterlimit strokeOpacity
strokeWidth surfaceScale systemLanguage tableValues targetX targetY textAnchor
textDecoration textLength textRendering to transform u1 u2 underlinePosition
underlineThickness unicode unicodeBidi unicodeRange unitsPerEm vAlphabetic
vHanging vIdeographic vMathematical values vectorEffect version vertAdvY
vertOriginX vertOriginY viewBox viewTarget visibility widths wordSpacing
writingMode x x1 x2 xChannelSelector xHeight xlinkActuate xlinkArcrole
xlinkHref xlinkRole xlinkShow xlinkTitle xlinkType xmlns xmlnsXlink xmlBase
xmlLang xmlSpace y y1 y2 yChannelSelector z zoomAndPan
```

## 合成事件
此参考指南记录了构成React事件系统一部分的SyntheticEvent包装器. 请参考有关事件处理的指南来了解更多

概述:
SyntheticEvent实例将被传递给你的事件处理函数, 它是浏览器的原生事件的跨浏览器包装器. 除兼容所有浏览器外, 它还拥有和浏览器原生事件相同的接口, 包括stopPropagation()和preventDefault()

如果因为某些原因, 当你需要使用浏览器底层事件时, 只需要使用nativeEvent属性来获取即可. 合成事件与浏览器的原生事件不同, 也不会直接映射到原生事件. 例如, 在onMouseLeave事件中event.nativeEvent将指向mouseout事件. 每个SyntheticEvent对象都包含以下属性
```
boolean bubbles
boolean cancelable
DOMEventTarget currentTarget
boolean defaultPrevented
number eventPhase
boolean isTrusted
DOMEvent nativeEvent
void preventDefault()
boolean isDefaultPrevented()
void stopPropagation()
boolean isPropagationStopped()
void persist()
DOMEventTarget target
number timeStamp
string type
```
Note: 从v17开始, e.persist()将不再生效, 因为SyntheicEvent不再放入事件池中.


Note: 从v0.14开始, 事件处理器返回false时, 不再阻止事件传递. 你可以酌情手动调用e.stopPropagation()或e.preventDefault()作为替代方案

### 支持的事件
React通过将事件normalize以让他们在不同浏览器中拥有一致的属性.
一下的事件处理函数在冒泡阶段被触发. 如需注册捕获阶段的事件处理函数, 则应为事件名添加Capture. 例如, 处理捕获阶段的点击事件请使用onClickCapture, 而不是onClick

### 参考
#### 剪切板事件
事件名
`onCopy onCut onPaste`
属性
`DOMDataTransfer clipboardData`

#### 复合事件
事件名:
`onCompositionEnd onCompositionStart onCompositionUpdatee`
属性
`string data`

#### 键盘事件
事件名:
`onKeyDown onKeyPress onKeyUp`
属性:
```
boolean altKey
number charCode
boolean ctrlKey
boolean getModifierState(key)
string key
number keyCode
string locale
number location
boolean metaKey
boolean repeat
boolean shiftKey
number which
```
key属性可以是DOM Level 3 Events spec里记录的任意值

#### 焦点事件
事件名:
`onFocus onBlur`
这些焦点事件在React DOM上的所有元素都有效, 不只是表单元素
属性:
`DOMEventTarget relatedTarget`

onFocus事件在元素(或其内部某些元素) 聚焦时被调用. 例如, 当用户点击文本输入框时, 就会调用该事件

onBlur事件在元素(或其内部某些元素) 失去焦点时被调用. 例如, 当用户在已聚焦的文本输入框外点击时, 就会被调用

监听焦点的进入与离开
你可以使用currentTarget和relatedTarget来区分聚焦和失去焦点是否来自元素外部. 这里有个DEMO, 你可以复制并本地允许, 它展示了如何监听一个子元素的聚焦, 元素本身的聚焦, 以及整个子树进入聚焦或离开焦点
```js
function Example() {
  return (
    <div
      tabIndex={1}
      onFocus={(e) => {
        if (e.currentTarget === e.target) {
          console.log('focused self');
        } else {
          console.log('focused child', e.target);
        }
        if (!e.currentTarget.contains(e.relatedTarget)) {
          // Not triggered when swapping focus between children
          console.log('focus entered self');
        }
      }}
      onBlur={(e) => {
        if (e.currentTarget === e.target) {
          console.log('unfocused self');
        } else {
          console.log('unfocused child', e.target);
        }
        if (!e.currentTarget.contains(e.relatedTarget)) {
          // Not triggered when swapping focus between children
          console.log('focus left self');
        }
      }}
    >
      <input id="1" />
      <input id="2" />
    </div>
  );
}
```
#### 表单事件
事件名
`onChange onInput onInvalid onReset onSubmit`

#### 通用事件
事件名:
`onError onLoad`

#### 鼠标事件
事件名
`onClick onContextMenu onDoubleClick onDrag onDragEnd onDragEnter onDragExit
onDragLeave onDragOver onDragStart onDrop onMouseDown onMouseEnter onMouseLeave
onMouseMove onMouseOut onMouseOver onMouseUp`
onMouseEnter和onMouseLeave事件从离开的元素向进入的元素传播, 不是正常的冒泡, 也没有捕获阶段
属性:
```
boolean altKey
number button
number buttons
number clientX
number clientY
boolean ctrlKey
boolean getModifierState(key)
boolean metaKey
number pageX
number pageY
DOMEventTarget relatedTarget
number screenX
number screenY
boolean shiftKey
```

#### 指针事件
事件名:
```
onPointerDown onPointerMove onPointerUp onPointerCancel onGotPointerCapture
onLostPointerCapture onPointerEnter onPointerLeave onPointerOver onPointerOut
```
onPointerEnter和onPointerLeave事件从离开的元素向进入的元素传播, 不是正常的冒泡, 也没有捕获阶段
属性:
在W3 spec 中定义的, 指针事件通过以下属性扩展了鼠标事件:
```
number pointerId
number width
number height
number pressure
number tangentialPressure
number tiltX
number tiltY
number twist
string pointerType
boolean isPrimary
```
React 故意不通过polyfill的方式适配其他浏览器,主要是符合标准的polyfill会显著增加react-dom的bundle大小.
如果你的应用要求指针事件, 我们推荐添加第三方的指针事件polyfill

#### 选择事件
事件名:
`onSelect`
#### 触摸事件
事件名
`onTouchCancel onTouchEnd onTouchMove onTouchStart`
属性
```
boolean altKey
DOMTouchList changedTouches
boolean ctrlKey
boolean getModifierState(key)
boolean metaKey
boolean shiftKey
DOMTouchList targetTouches
DOMTouchList touches
```

#### UI事件
事件名
`onScroll`
Note
从React17开始, onScroll事件在React中不再冒泡. 这与浏览器的行为一致, 并且避免了当一个嵌套且可滚动的元素在其父元素触发事件时造成混乱.

属性: 
```
number detail
DOMAbstractView view
```

#### 滚轮事件
事件名
`onWheel`
属性
```
number deltaMode
number deltaX
number deltaY
number deltaZ
```
#### 媒体事件
事件名
```
onAbort onCanPlay onCanPlayThrough onDurationChange onEmptied onEncrypted
onEnded onError onLoadedData onLoadedMetadata onLoadStart onPause onPlay
onPlaying onProgress onRateChange onSeeked onSeeking onStalled onSuspend
onTimeUpdate onVolumeChange onWaiting
```
#### 图像事件
事件名
`onLoad onError`

#### 动画事件
事件名
`onAnimationStart onAnimationEnd onAnimationIteration`
属性
```
string animationName
string pseudoElement
float elapsedTime
```

#### 过渡事件
事件名: onTranstionEnd
属性
```
string propertyName
string pseudoElement
float elapsedTime
```

#### 其他时间
事件名
`onToggle`

## Test Utilities
ReactTestUtils 可搭配你所选的测试框架, 轻松实现React组件测试. 在Facebook内部, 我们使用Jest来轻松实现JavaScript测试. 你可以从Jest官网的Reat教程中了解如何使用它

## Hook
自定义hook更新是一种约定而不是功能. 如果函数的名字以“use”开头并调用其他Hook, 我们就说这是一个自定义hook, useSomething的命名约定可以让我们的linter插件在使用hook的代码中找到bug.

### useEffect
Note 
与componentDidMount或componentDIdUpdate不同, 使用useEffect调度的effect不会阻塞浏览器更新屏幕, 这让你的应用看起来响应更快. 大多数情况下, effect不需要同步地执行. 在个别情况下(例如测量布局), 有单独的useLayoutEffect Hook供你使用, 其API与useEffect相同