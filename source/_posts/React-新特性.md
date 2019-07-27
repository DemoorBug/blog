---
title: React 新特性
date: 2019-07-18 16:47:46
tags: [react]
categories: 学习
---

项目目录
待填

<!-- more -->
# 一些知识点
## SPA/MPA
单页应用/多页应用
MPA和SPA没有本质区别，SPA需要改造才能变为MPA,大网站都是这么搞的
## PWA 渐进式网络应用
浏览器不支持pwa的时候就会退化为普通的应用，所以称之为渐进式网络应用。
如果浏览器支持PWA，就可以控制静态资源缓存，即便我们的设备没有联网，也可以用缓存来运行页面，提供了强大的离线访问能力，除了离线访问，还可以优化载入速度，PWA是前端近年来的革命性进化，移动端支持良好。vue官网就用了这种功能

# React 新特性
## Context
定义：Context 提供了一种方式，能够让数据在组件树中传递而不必一级一级手动传递
API: createContext(defaultValue?)
解决问题：编程效率
App.js:
```js
import React, { createContext } from 'react';
import './App.css';

const BatteryContext = createContext(); //首先是创建，默认值是找不到Provider时候使用，一般是单元测试会用到
const OnlineContext = createContext();

class Leaf extends React.Component {
  render () {
    return (
      <BatteryContext.Consumer> //可以无限嵌套，没有先后之分，注意嵌套规则即可
        {
          battery => (
            <OnlineContext.Consumer>
              {
                online => <h1>Battery: {battery}, online: {online ? '联网':'未联网'}</h1>
              }
              // 布尔值可以这样显示 String(online)
            </OnlineContext.Consumer>
          )
        }
      </BatteryContext.Consumer>
    )
  }
}

class Middle extends React.Component {
  render () {
    return <Leaf/>;
  }
}

class App extends React.Component {
  state = {
    battery: 60,
    online: false
  };
  render () {
    const { battery, online } = this.state
    return (
      <BatteryContext.Provider value={battery}>
        <OnlineContext.Provider value={online}>
          <button
            type="button"
            onClick={() => this.setState({battery: battery - 1})}
          >
            Press
          </button>
          <button
            type="button"
            onClick={() => this.setState({online: !online})}
          >
            Online
          </button>
          <Middle />
        </OnlineContext.Provider>
      </BatteryContext.Provider>
    );
  }
}

export default App;

```

## ContextType

结合Context代码使用
```js
class Leaf extends React.Component {
  static contextType = BatteryContext
  render () {
    const battery = this.context
    return (
      <h1>Battery: {battery}</h1>
    )
  }
}
```
一个Context确实挺方便，多个是否就无法使用了呢？
看到4-4节的时候老师有说，确实是只能指向一个，但是可以用useContext 解决这个问题，写法还会更简单

## lazy
所有的细节都在注释里面

app.js
```js
import React, { lazy, Suspense } from 'react';
import './App.css';

const About = lazy(() => import(/* webpackChunkName: "about" */ './About.jsx'))

// 错误捕获，如果手动将about.chunk.js 右键Block request URL，就会报错，我们必须手动处理错误。
// 借助 ErrorBoundary，他的实现原理就是用react的生命周期钩子，componentDidCatch
// 不过页面还是会报错，老师的报错位置是控制台，我直接就是页面

class App extends React.Component {
  state = {
    hasError: false
  };

  // componentDidCatch () {
  //   this.setState({
  //     hasError: true
  //   })
  // }
  // ErrorBoundary的另一种写法

  static getDerivedStateFromError () { // 一旦遇到错误，他就会返回一个新的state数据，并合并到组件的state中
    return {
      hasError: true
    }
  }

  render () {
    if (this.state.hasError) {
      return (
        <div>加载错误，可能是网络请求问题</div>
      )
    }
    return (
      <div>
        <Suspense fallback={<div>loading</div>}>
          <About />
        </Suspense>
      </div>
    );
  }
}

export default App;

```

About.jsx
```js
import React, { Component } from 'react'

class About extends Component {
  render () {
    return (
      <div>About</div>
    )
  }
}

export default About

```
## Suspense
都在上面的代码里面

## memo
解决父组件更改,子组件重新渲染问题

```js
import React, { PureComponent, memo } from 'react';
import './App.css';

// memo 解决方案
const Foo = memo(function Foo (props) {
  console.log(props.name)
  return null;
})

// 另一种解决方案PureComponent
// 有局限性，只有传入属性的对比，如果内部发生什么变化，就么得用了
// 比如传入对象就算修改，也不会更新视图。还有传入内敛callback(<Foo cb={() => {}}></Foo>)，这样每次都会渲染

// callback () {}
// <Foo cb={this.callback}></Foo>
// 解决callback this指向问题 callback = () => {} ,这就是一个比较完美的写法了，不过又听说箭头函数优化不好。
// class Foo extends PureComponent {
//   render () {
//     console.log(this.props.name)
//     return null;
//   }
// }

// 回调解决方案
// class Foo extends React.Component {
//   //　是否决定重新渲染
//   shouldComponentUpdate (nextProps, nextState) {
//     if (nextProps.name === this.props.name) {
//       return false
//     }
//     return true
//   }
//   render () {
//     console.log(this.props.name)
//     return null;
//   }
// }

class App extends React.Component {
  state = {
    num: 0
  }
  render () {
    return (
      <div>
        <button onClick={()=>{this.setState({num: this.state.num + 1})}}>{this.state.num}</button>
        <Foo name="Foo"></Foo>
      </div>
    )
  }
}

export default App;

```

# State Hooks
# useState
所有的hooks函数都应该以use开头
```js
function App (props) {
  let [num, setNum] = useState(() => {
    console.log('inall')
    return props.defaultCount || 0
  })

  return (
    <div>
      <button onClick={() => setNum(num + 1)}>
        {num}
      </button>
    </div>
  )
}
```
## Effect Hooks
useEffect 代替 componentDidMount, componentDidUpdate, componentWillUnmount
灵活运用,就可以达到上面生命周期钩子的效果

```js
import React, { useState, useEffect } from 'react';
import './App.css';


function App (props) {
  // let num, setNum;
  // let Main, setMain;
  let [title, setNum] = useState(0)
  let [size, setSize] = useState({
    width: document.documentElement.clientWidth,
    height: document.documentElement.clientHeight
  })

  const onResize = () => {
    setSize({
      width: document.documentElement.clientWidth,
      height: document.documentElement.clientHeight
    })
  }
  useEffect(() => {
    document.title = title
  })
  useEffect(() => {
    window.addEventListener('resize', onResize, false)
    return () => { //回调清理函数
      console.log('remove');
      window.removeEventListener('resize', onResize, false)
    }
  }, []) // 数组的每一项都不变,才会阻止里面的内容更改
  return (
    <div>
      <button onClick={() => setNum(title + 1)}>
        {title}, {size.width}, {size.height}
      </button>
    </div>
  )
}

export default App;
```
## Hooks 下的 Context
可以说是很简单了
不要滥用context, 破坏组件独立性

App.js
```js
import React, { useState, createContext , useContext} from 'react';
import './App.css';

const BatteryContext = createContext();

function Foo() {
  const title = useContext(BatteryContext)
  return (
    <div>{title}</div>
  )
}

function App (props) {
  const [title, setNum] = useState(0)

  return (
    <div>
      <button onClick={() => setNum(title + 1)}>
        Add
      </button>
      <BatteryContext.Provider value={title}>
        <Foo />
      </BatteryContext.Provider>
    </div>
  )
}

export default App;

```
## useMemo
这是掘金看到的写法，老师的写法有错，现在用会警告，而且没有视频中的效果
纠结了半小时，原来是要配合memo，听课没认真的后果。
```js
import React, { useState, useMemo } from 'react';
import './App.css';


function Foo(props) {
  console.log('渲染');
  return (
    <div title={props.title}>{props.title}</div>
  )
}

function Coo(props) {
  console.log('Coo');
  return (
    <div>{props.coo}</div>
  )
}

function App (props) {
  const [title, setNum] = useState(0)
  const [coo, setCoo] = useState(2)

  const double = useMemo(() => <Foo title={title}/>, [title]);
  const douCoo = useMemo(() => <Coo coo={coo}/>, [coo])

  return (
    <div>
      <button onClick={() => setNum(title + 1)}>
        Add
      </button>
      <button onClick={() => setCoo(coo + 1)}>
        C
      </button>
        {double}
        {douCoo}
        <div>{double}</div>
    </div>
  )
}

export default App;

```

配合 memo 使用
```js
import React, { useState, useMemo, memo, useCallback } from 'react';
import './App.css';


const Foo = memo(function Foo(props) {
  console.log('渲染');
  return (
    <h1 onClick={props.onClick}>{props.title}</h1>
  )
})

function App (props) {
  const [title, setNum] = useState(0)
  const [lest, setLest] = useState(0)

  const fun = useMemo(() => { // 如果是一个普通的箭头函数，每次都会是一个新的句柄，只有这样才会传入固定的句柄
    return () => {
      console.log('useMemo');
    }
  }, [])
  // 如果useMemo返回的是函数，可以直接用useCallback，来省略顶层的函数
  // 这两种写法等价
  const fun = useCallback(() => {
    console.log('useCallback')
  }, [])

  // 更新状态, 依赖的两个变量都要写到状态里面，其实官方说，可以保证每次setState返回的同一个句柄
  // const fun = useCallback(() => {
  //   console.log('callback')
  //   setLest((le) => le + 1)
  // }, [])
  // 另一种更新状态写法
  // const fun = useCallback(() => {
  //   console.log('callback')
  //   setLest(lest + 1)
  // }, [lest])

  const num = useMemo(() => {
    return title * 2
  }, [title === 3]) // 这么写会有报错，不过可以实现

  return (
    <div>
      <button onClick={() => setNum(title + 1)}>
        Add
      </button>-
      <button onClick={() => setLest(lest + 1)}>
        lest
      </button>
      <Foo onClick={fun} title={num}></Foo>
    </div>
  )
}

export default App;

```
## useRef
class 的时候可以使用 String Ref, Callback Ref, CreateRef
hooks 使用 useRef

```js
import React, { useState, PureComponent,useMemo, useCallback, useRef, useEffect } from 'react';
import './App.css';

// 因为函数无法创建实例，所以必须用class来实现，原来hooks不能完全代替奥
// const Foo = memo(function Foo(props) {
//   console.log('渲染');
//   return (
//     <h1 onClick={props.onClick}>{props.title}</h1>
//   )
// })

// 用PureComponent代替memo
class Foo extends PureComponent {
  mems () { // 第一种使用场景
    console.log(this.props.title);
  }
  render () {
    console.log('渲染');
    return (
      <h1 onClick={this.props.onClick}>{this.props.title}</h1>
    )
  }
}

function App (props) {
  const [title, setNum] = useState(0)
  const [lest, setLest] = useState(0)
  const contRef= useRef()
  const it = useRef()

  const fun = useCallback(() => {
    console.log('callback')
    setLest((le) => le + 1)

    contRef.current.mems(); // 第一种使用场景
  }, [contRef])

  const num = useMemo(() => {
    return title * 2
  }, [title]) // 这么写会有报错，不过可以实现

  useEffect(() => {
    // 如果是在App函数中声明变量，每次都会执行这个变量，就会导致无法清除it,就必须全局创建，state好像也是可以解决
    it.current = setInterval(() => { // Ref的第二种使用场景
      setNum(title => title +1)
    }, 1000)
  }, [])
  useEffect(() => {
    if (num >= 10) {
      clearInterval(it.current)
    }
  })

  return (
    <div>
      <button onClick={() => setNum(title + 1)}>
        Add
      </button>-
      <button onClick={() => setLest(lest + 1)}>
        {lest}
      </button>
      <Foo onClick={fun} ref={contRef} title={num}></Foo>
    </div>
  )
}

export default App;

```
