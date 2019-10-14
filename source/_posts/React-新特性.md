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

他娘的，这么简单，就是透传，和上面定义的是一个意思，在src/ticket/Candidate.jsx中有用到，很简单、很方便。

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
## 自定义hooks，达到状态逻辑复用
```js
import React, { useState, useMemo, useRef, useEffect, useCallback } from 'react';
import './App.css';



function useFoo(count) {
  const {height, width} = useSize()
  return (
    <h1>{count}, {height}, {width}</h1>
  )
}

function useCount() {
  const [title, setNum] = useState(0)
  const it = useRef()

  const num = useMemo(() => {
    return title * 2
  }, [title]) // 这么写会有报错，不过可以实现

  useEffect(() => {
    it.current = setInterval(() => { // Ref的第二种使用场景
      setNum(title => title +1)
    }, 1000)
  }, [])
  useEffect(() => {
    if (num >= 10) {
      clearInterval(it.current)
    }
  })
  return [title, setNum, num]
}

function useSize() {
  const [size, setSize] = useState({
    width: document.documentElement.clientWidth,
    height: document.documentElement.clientHeight
  });
  // const onResize = () => {
  //   setSize({
  //     width: document.documentElement.clientWidth,
  //     height: document.documentElement.clientHeight
  //   })
  // }
  const onResize = useCallback(() => { // 老师也是一笔带过，没有细讲为什么要用useCallback(), 我也记不起来了。算一个坑吧
    setSize({
      width: document.documentElement.clientWidth,
      height: document.documentElement.clientHeight
    })
  }, [])
  useEffect(() => {
    window.addEventListener('resize', onResize, false)
    return () => {
      window.removeEventListener('resize', onResize, false)
    }
  }, [onResize])

  return size
}

function App (props) {
  const [title, setNum, num] = useCount()
  const Counter = useFoo(num)
  const size = useSize()
  return (
    <div>
      <button onClick={() => setNum(title + 1)}>
        Add,{size.width},{size.height}
      </button>
      {Counter}
    </div>
  )
}

export default App;
```
## hooks 使用法则
顶层调用hooks函数：
不能在循环语句，条件语句或者是嵌套函数中调用hooks函数
仅在函数组件和自定义hooks函数中，调用hooks函数

## hooks使用问题
### 生命周期的问题：
getDerivedStateFromProps 可以用hooks代替
```js
class App extends React.component {
  state = {
    ovflive: false
  }
  static getDerivedStateFromProps(props) {
    if (props.num > 10) {
      return {
        ovflive: true
      }
    }
  }
}

// hooks写法
function App (props) {
  const [num, setNum] = useState(false)
  if (props.num > 10) {
    setNum(true) // 不用担心性能问题，这个setNum是在React操作dom之前完成的
  }
}
```
### shouldComponentUpdate
hooks 代替 memo

### componentDidMount componentDidUpdate componentWillUnmount
hooks 写法
```js
function App () {
  useEffect(() => {
    // componentDidMount
    return () => {
      // componentWillUnmount
    }
  }, []);
  let renderCounter = useRef(0)
  renderCounter.current++;

  useEffect(() => {
    if (renderCounter > 1) {
      // componentDidUpdate
    }
  })
}
```

### getSnapshotBeforeUpdate componentDidCatch getDerivedStateFromError
目前hooks无法实现，函数组件目前还无法取代类组件



### 类实例成员如何映射到hooks
```js
class App {
  it = 0;
}

function App() {
  const it = useRef(0) // 初始值不能传入函数
}
```

### hooks如何获取历史props和state
了解了了解了，如果不用useRef每次组件重新渲染值都会初始化为0，只有用useRef值才能保持不变，还有就是useEffect的优先级比较低，怎么比喻呢。，写下来把(0表示最先，依次)
次序有问题，下面做了修改
```js
function Counter() {
  const [count, setCount] = useState(0) // 0
  const prevCountRef = useRef() // 0

  useEffect(() => { // 2
    prevCountRef.current = count // 保存上一次的count值，因为ref不受重新渲染的影响，因此可以从下一次渲染中取出count
  })
  const prevCount = prevCountRef.current // 1

  return <h1>Now: {count}, before: {prevCount}</h1> // 3
}
```

上面的次序有问题，这里重新更改, useEffect 居然这么晚执行
```js
function Counter() {
  const [count, setCount] = useState(0) // 0
  const prevCountRef = useRef() // 0

  useEffect(() => { // 2
    prevCountRef.current = count
  })
  const prevCount = prevCountRef.current // 0

  return <h1>Now: {count}, before: {prevCount}</h1> // 1
}
```
### 如何强制更新一个hooks组件
class中有一个forceUpdate, 不过hooks 我们可以用其他方法其代替

```js
function Counter() {
  const [count, setCount] = useState(0)
  const [updater, setUpdater] = useState(0)

  function forceUpdate() {
    setUpdater(updater => updater + 1) // 更新updater 就是间接更新了hooks组件
  }

  const prevCountRef = useRef()

  useEffect(() => {
    prevCountRef.current = count
  })
  const prevCount = prevCountRef.current

  return <h1>Now: {count}, before: {prevCount}</h1>
}
```
## Redux
Redux 三大原则
单一数据源
状态不可变
纯函数修改数据

### 实现的一个todolist应用。很简单，样式以及源码全部在train-ticket的src/TodoListSoundCode目录下
这里放上app.js代码部分，方便查阅
```js
import React, { useState, useCallback, useRef, useEffect, memo } from 'react';
import './App.css';

let idSeq = Date.now()

const Control = memo(function Control(props) {
  const { addTodo } = props
  const inputRef = useRef()

  const onSubmit = (e) => { // 没有像任何子组件传递，所以就没有必要包裹callback
    e.preventDefault()

    const newText = inputRef.current.value.trim()

    if (newText.length === 0) {
      return
    }
    addTodo({
      id: ++idSeq,
      text: newText,
      complate: false
    })

    inputRef.current.value = ''
  }

  return (
    <div className="control">
      <h1>todos</h1>
      <form onSubmit={onSubmit}>
        <input
          type="text"
          ref={inputRef}
          className="new-todo"
          placeholder="what needs to be done?"
        />
      </form>
    </div>
  )
})

const TodoItem = memo(function TodoItem(props) {
  const {
    todo: {
      id,
      text,
      complate
    },
    toggleTodo,
    removeTodo
  } = props
  const onChange = () => {
    toggleTodo(id)
  }
  const onRemove = () => {
    removeTodo(id)
  }
  console.log('0');
  return (
    <li className="todo-item">
      <input
        type="checkbox"
        onChange={onChange}
        checked={complate}
      />
      <label className={complate ? 'complate' : ''}>{text},{String(complate)}</label>
      <button onClick={onRemove}>&#xd7;</button>
    </li>
  )
})

const Todos = memo(function Todos(props) {
  const { todos, toggleTodo, removeTodo } = props
  return (
    <ul>
      {
        todos.map(todo => {
          return <TodoItem
            key={todo.id}
            todo={todo}
            toggleTodo={toggleTodo}
            removeTodo={removeTodo}
          />
        })
      }
    </ul>
  )
})
const LS_KEY = '$-todos_';
function TodoList() {
  const [todos, setTodos] = useState([])

  const addTodo = useCallback((todo) => {
    setTodos(todos => [...todos, todo])
  }, [])
  const removeTodo = useCallback((id) => {
    setTodos(todos => todos.filter(todo => {
      return todo.id !== id
    }))
  }, [])
  const toggleTodo = useCallback((id) => {
    setTodos(todos => todos.map(todo => {
      return todo.id === id
        ? {
          ...todo,
          complate: !todo.complate
        }
        : todo;
    }))
  }, [])

  // 下面两个副作用都是来实现本地数据存储的
  useEffect(() => {
    const todos = JSON.parse(localStorage.getItem(LS_KEY) || '[]')
    setTodos(todos)
  }, [])
  useEffect(() => {
    localStorage.setItem(LS_KEY, JSON.stringify(todos))
  }, [todos])
  return (
    <div className="todo-list">
      <Control
        addTodo={addTodo}
      />
      <Todos
        removeTodo={removeTodo}
        toggleTodo={toggleTodo}
        todos={todos}
      />
    </div>
  )
}

export default TodoList;

```
### Dispatch 与 Action
这不就是我苦苦寻找的vuex里面的解构为什么可以...dispatch的原因了吗，帅呆了奥，原来是这么写一个函数的
addTodo = (payload) => dispatch(createAdd(payload))
把传入的对象转换为下面这种格式，然后通过解构的方式传递给组件，帅
{
   addTodo: dispatch Function
}
```js
import React, { useState, useCallback, useRef, useEffect, memo } from 'react';
import './App.css';
import {
  createAdd,
  createSet,
  createRemove,
  createToggle
} from './actions'

let idSeq = Date.now()

// 这不就是我苦苦寻找的vuex里面的解构为什么可以...dispatch的原因了吗，帅呆了奥，原来是这么写一个函数的
// addTodo = (payload) => dispatch(createAdd(payload))
// 把传入的对象转换为下面这种格式，然后通过解构的方式传递给组件，帅
// {
//    addTodo: dispatch Function
// }
function bindActionCreators(actionCreators, dispatch) {
  const ret = {}

  for (let key in actionCreators) {
    ret[key] = function (...args) {
      const actionCreator = actionCreators[key]
      const action = actionCreator(...args)
      dispatch(action)
    }
  }

  return ret
}

const Control = memo(function Control(props) {
  const { addTodos } = props
  const inputRef = useRef()

  const onSubmit = (e) => { // 没有像任何子组件传递，所以就没有必要包裹callback
    e.preventDefault()

    const newText = inputRef.current.value.trim()

    if (newText.length === 0) {
      return
    }
    addTodos({
      id: ++idSeq,
      text: newText,
      complate: false
    })
    inputRef.current.value = ''
  }

  return (
    <div className="control">
      <h1>todos</h1>
      <form onSubmit={onSubmit}>
        <input
          type="text"
          ref={inputRef}
          className="new-todo"
          placeholder="what needs to be done?"
        />
      </form>
    </div>
  )
})

const TodoItem = memo(function TodoItem(props) {
  const {
    todo: {
      id,
      text,
      complate
    },
    remove,
    toggle
  } = props
  const onChange = () => {
    toggle(id)
  }
  const onRemove = () => {
    remove(id)
  }
  return (
    <li className="todo-item">
      <input
        type="checkbox"
        onChange={onChange}
        checked={complate}
      />
      <label className={complate ? 'complate' : ''}>{text},{String(complate)}</label>
      <button onClick={onRemove}>&#xd7;</button>
    </li>
  )
})

const Todos = memo(function Todos(props) {
  const { todos, remove, toggle } = props
  return (
    <ul>
      {
        todos.map(todo => {
          return <TodoItem
            key={todo.id}
            todo={todo}
            remove={remove}
            toggle={toggle}
          />
        })
      }
    </ul>
  )
})
const LS_KEY = '$-todos_';
function TodoList() {
  const [todos, setTodos] = useState([])

  const dispatch = useCallback((action) => {
    const { type, payload } = action
    switch (type) {
      case 'set':
        setTodos(payload)
        break;
      case 'add':
        setTodos(todos => [...todos, payload])
        break;
      case 'remove':
        setTodos(todos => todos.filter(todo => {
          return todo.id !== payload
        }))
        break;
      case 'toggle':
        setTodos(todos => todos.map(todo => {
          return todo.id === payload
            ? {
              ...todo,
              complate: !todo.complate
            }
            : todo;
        }))
        break;
      default:
    }
  }, [])

  useEffect(() => {
    const todos = JSON.parse(localStorage.getItem(LS_KEY) || '[]')
    dispatch(createSet(todos))
  }, [dispatch])
  useEffect(() => {
    localStorage.setItem(LS_KEY, JSON.stringify(todos))
  }, [todos])
  return (
    <div className="todo-list">
      <Control
        {
          ...bindActionCreators({
            addTodos: createAdd
          }, dispatch)
        }
      />
      <Todos
        {
          ...bindActionCreators({
            remove: createRemove,
            toggle: createToggle
          }, dispatch)
        }
        todos={todos}
      />
    </div>
  )
}

export default TodoList;

```
### reducer 这边问题很多啊
```js
import React, { useState, useCallback, useRef, useEffect, memo } from 'react';
import './App.css';
import {
  createAdd,
  createSet,
  createRemove,
  createToggle
} from './actions'

let idSeq = Date.now()

// 这不就是我苦苦寻找的vuex里面的解构为什么可以...dispatch的原因了吗，帅呆了奥，原来是这么写一个函数的
// addTodo = (payload) => dispatch(createAdd(payload))
// 把传入的对象转换为下面这种格式，然后通过解构的方式传递给组件，帅
// {
//    addTodo: dispatch Function
// }
function bindActionCreators(actionCreators, dispatch) {
  const ret = {}

  for (let key in actionCreators) {
    ret[key] = function (...args) {
      const actionCreator = actionCreators[key]
      const action = actionCreator(...args)
      dispatch(action)
    }
  }

  return ret
}

const Control = memo(function Control(props) {
  const { addTodos } = props
  const inputRef = useRef()

  const onSubmit = (e) => { // 没有像任何子组件传递，所以就没有必要包裹callback
    e.preventDefault()

    const newText = inputRef.current.value.trim()

    if (newText.length === 0) {
      return
    }
    addTodos({
      id: ++idSeq,
      text: newText,
      complate: false
    })
    inputRef.current.value = ''
  }

  return (
    <div className="control">
      <h1>todos</h1>
      <form onSubmit={onSubmit}>
        <input
          type="text"
          ref={inputRef}
          className="new-todo"
          placeholder="what needs to be done?"
          />
      </form>
    </div>
  )
})

const TodoItem = memo(function TodoItem(props) {
  const {
    todo: {
      id,
      text,
      complate
    },
    remove,
    toggle
  } = props
  const onChange = () => {
    toggle(id)
  }
  const onRemove = () => {
    remove(id)
  }
  return (
    <li className="todo-item">
      <input
        type="checkbox"
        onChange={onChange}
        checked={complate}
        />
      <label className={complate ? 'complate' : ''}>{text},{String(complate)}</label>
      <button onClick={onRemove}>&#xd7;</button>
    </li>
  )
})

const Todos = memo(function Todos(props) {
  const { todos, remove, toggle } = props
  return (
    <ul>
      {
        todos.map(todo => {
          return <TodoItem
            key={todo.id}
            todo={todo}
            remove={remove}
            toggle={toggle}
            />
        })
      }
    </ul>
  )
})

function reducer(state, action) {
  const { type, payload } = action;
  const { todos, incrementCount } = state
  switch (type) {
    case 'set':
      return {
        ...state,
        todos: payload,
        incrementCount: incrementCount + 1
      }
    case 'add':
      return {
        ...state,
        todos: [...todos, payload],
        incrementCount: incrementCount + 1
      }
    case 'remove':
      return {
        ...state,
        todos: todos.filter(todo => {
          return todo.id !== payload
        })
      }
    case 'toggle':
      return {
        ...state,
        todos: todos.map(todo => {
          return todo.id === payload
          ? {
            ...todo,
            complate: !todo.complate
          }
          : todo;
        })
      }
    default:
      return state
  }
}


const LS_KEY = '$-todos_';
function TodoList() {
  const [todos, setTodos] = useState([])
  const [incrementCount, setIncrementCount] = useState(0)

  const dispatch = useCallback((action) => {
    const state = {
      todos,
      incrementCount
    }
    const setters = {
      todos: setTodos,
      incrementCount: setIncrementCount
    }
    const newState = reducer(state, action)
    for (var key in newState) {
      setters[key](newState[key])
    }
  }, [todos, incrementCount])

  useEffect(() => {
    const todos = JSON.parse(localStorage.getItem(LS_KEY) || '[]')
    dispatch(createSet(todos))
  }, [])
  useEffect(() => {
    localStorage.setItem(LS_KEY, JSON.stringify(todos))
  }, [todos])
  return (
    <div className="todo-list">
      <Control
        {
          ...bindActionCreators({
            addTodos: createAdd
          }, dispatch)
        }
        />
      <Todos
        {
          ...bindActionCreators({
            remove: createRemove,
            toggle: createToggle
          }, dispatch)
        }
        todos={todos}
        />
    </div>
  )
}

export default TodoList;

```

以上代码这一块编辑器提示必须依赖dispatch，否则就删除数组，但是依赖后就会出现bug，代码会进入无限循环，莫名其妙，不过老师后面的异步actions好像是可以避免这个问题，但是我没有跟进，直接进入了PWA应用
```js
useEffect(() => {
  const todos = JSON.parse(localStorage.getItem(LS_KEY) || '[]')
  dispatch(createSet(todos))
}, [])
```
## PWA (Progressive Web App)
渐进式网络应用
### Service Worker 服务器工作线程
常驻内存运行
代理网络请求
依赖HttpS

### Promise
优化回调地狱
async/await 语法同步化
### fetch
比XMLHttpRequest更简洁
Promise风格
### cache API
支持资源的缓存系统
缓存(css/scritp/image)
依赖Service Worker代理网络请求
### Notification API
消息推送
依赖用户授权
适合在Service Worker中推送


## 删除除serviceWorker.js以外的所有文件
```bash
ls | grep -v serviceWorker.js | xargs rm
```
这句代码也太帅了？从来没用过哎

## 小技巧
**生成26个字母表**
```js
const alphabet = Array.from(new Array(26), (eie, index) => {
  return String.fromCharCode(65 + index)
})
```
**通过data来进行元素的锚跳转**
```html
<div data-cate="X">X</div>
<script>
document.querySelector(`[data-cate='X']`)
  .scrollIntoView()
</script>
```
**传参如果是对象可以解构：**
```jsx
const bash = {
  name,
  bash,
  hello
}

return (
  <div {...bash}></div>
)
```

**icon小图标的&#x在js中要替换为'\u'**

**delete操作符居然可以这样用-**
in运算符：
src/query/Bottom.jsx
```js
const toggle = useCallback((value) => {
  const newCheckedMap = {...checkedMap}
  if (value in checkedMap) {
    delete newCheckedMap[value]
  } else {
    newCheckedMap[value] = true
  }
  update(newCheckedMap)
}, [checkedMap, update])
```

**今天用到了`Object.assign()`**
目前根据代码推断，作用应该等同于:
```js
const data = {name:1}

data = {...data, msg: "helo"}

// 等同于

Object.assign(data, {
  msg: "helo"
})

```

**Candidate.jsx中还写了个选项卡，很简单，很简单**

## 没有用过的css
**实现的效果就是拖动到顶部的时候会变成固定定位，下一个同样属性的元素划过会替换掉。蛮神奇的**
```css
{
  position: sticky;
}
```
**readonly**
input的一个属性，加了后不能填写，初步理解

## 日期组件很顶，以后可以用到估计
common/DeteSelector.jsx

这个日期选择组件实在是帅的一批，react居然要拆这么细的组件，日期组件原来这么简单，感觉下一次做就不会有所畏惧了，结尾老师说的注意点，什么鬼，听不明白，hide？

## 开发中所用到的npm安装的模块总结
**prop-types 校验传入属性值的类型**
具体在common/header.jsx 文件中使用
classnames 动态类、没有这个的话可以这么写
没有：
```jsx
<div className={['city-selector', (!show) && 'hidden'].filter(Boolean).join(' ')}></div>
```
**用classnames**
```jsx
import classnames from 'classnames'
<div className={classnames('city-selector', {
  hidden: !show
  })}></div>
```
**day.js** 仅2K大小，和Moment.js一样的api，nice啊，虽然没有用过Monment

**urijs**
解析用get提交的url数据，这个居然也要库，怎么什么都要库啊，自己写不行吗。
```js
import URI from 'urijs'
const {dispatch} = props
useEffect(() => {
    const queries = URI.parseQuery(window.location.search)

    const {
      from,
      to,
      date,
      highSpeed
    } = queries
    dispatch(setFrom(from))
    dispatch(setTo(to))
    dispatch(setDeparDate(h0(dayjs(date).valueOf())))
    dispatch(setHighSpeed(highSpeed === 'true'))
}, [])
```
**left-pad** 这个模块就11行代码，我真的服了，现在有现成的支持,String.padStart()
这几集的视频没了，只能自己看源码，还行吧，挺简单的，里面操作滚动条技巧还挺厉害的，掌握原理了，我自己写还真写不出来，艾，基础太薄弱了。fuck
```ts
String(index: number).padStart(2, '0')
```

## webpack 的 MPA 多页网页设置
9-2 节必须更改config/webpack.config.js下的：
```js
output: {
  /* 142 */ filename: isEnvProduction
  /* 143 */ ? 'static/js/[name].[chunkhash:8].js'
  /* 144 */ : isEnvDevelopment && 'static/js/build.js'
}
```
更改为: 'static/js/[name].js'
```js
output: {
  /* 142 */ filename: isEnvProduction
  /* 143 */ ? 'static/js/[name].[chunkhash:8].js'
  /* 144 */ : isEnvDevelopment && 'static/js/[name].js'
}
```
## 应该补充的知识
**给组件传自定义dom**
这块还不知道其他用法，比如传多个dom。
````jsx
function Detail(props) {
  return (
    <div>
		{ props.children }
    </div>
  )
}

function App(){
  return (
    <Detail>
      // 这里可以自己定义
      <span></span>
    </Detail>
  )
}

```
**for of 以及 object.keys() 的使用**
以前还专门记过，他娘的，怎么就忘了呢
src/order/actions.js
```js
const passengers = [
  {id: 0, ticketType: 'abult'},
  {id: 1, child: ''}
]
for (let passenger of passengers) {
  const keys = Object.keys(passenger)
  for (let key of keys) {
    if (!passenger[key]) {
      return
    }
  }
  if (passenger.ticketType === 'adult') {
    adultFound = passenger.id
  }
}
```
# 结束语
总体来说还是可以的，hooks比想象的要简单要好用的多

12.1 代码规范和格式化，git提交格式化，使用Prettier增强eslist的格式化能力
12.2 代码部署以及PWA应用所需的，serviceWorker.js 的应用
