---
title: vuesenior
date: 2018-12-13 22:57:37
tags: [vue,SSR,Koa2]
categories: [vue]
---

vue全家桶+SSR+Koa2全栈开发美团网
------------------------------------
项目地址：[vue全家桶+SSR+Koa2全栈开发美团网](https://github.com/DemoorBug/vuesenion)



安装vue-cli
------------------------------------
vue脚手架创建项目环境
```bash
npm install -g @vue/cli
vue -V
vue create 项目名称
```
查看项目remote
```bash
git remote -v
git remote remove <name>

```


git 本地创建目录命令
```bash
线上创建项目
mkdir 项目名称
cd 项目名称
git init 
vi README.md
git add README.md
git commit -m "上传简介"
git remote add origin git@github.com:DemoorBug/vuesenior.git

git push -u origin master      -u代表了关联本地分支和线上分支的master分支，以后就可以直接git push了

```

Koa2学习
--------------
首先要装一个脚手架，我以前学node的时候老师用的express，今年这个koa2最火
koa-generator
安装
```bash
npm install -g koa-generator
```
创建项目
```
koa2 project 默认是用这个引擎安装的
想用e js引擎
koa2 -e project
```

运行项目
```
cd project
npm install
npm run dev
```

async和await语法
---------------------
这就是prmis，的一个写法，很简单


koa2中间件
--------------------
中间件一直是我没搞懂的

[koa2_api](https://koa.bootcss.com)

大致理解就是一个环，先进来，在出去都可以执行到我，
比如执行顺序是这样的:
m1
m2
m3
出去的执行顺序是这样的:
m3
m2
m1

所以谁前谁后执行无所谓，出去的时候还会自我检查，我的优先级会被提高

中间件代码

```js
function pv (ctx) {
  global.console.log('pv')
}

module.exports = function() {
  return async function(ctx, next) {
    pv(ctx)
    await next()
    global.console.log('pv end')
  }
}
```


koa路由
--------------

#### cookies
ctx.cookies.set('pvd', Math.random())
ctx.cookies.get('pvd')


mongoose
--------------

```bash
which   搜索命令的命令。搜索命令所在路径及别名

which mongod
```

[mongoose中文文档](https://mongoose.shujuwajue.com/)


#### 研究了一下windows下的mongodb启动，这里总结一下
键入的命令
```bash
mongod --dbpath "f:\mo\data" --logpath "f:\mo\log\db.log" --install --serviceName "mongodb" --logappend --directoryperdb
```

这里面的启动名字是mongodb
```bash
net start mongodb
```
我看有些人的教程直接是   
```bash
.\mongod --dbpath="E:\MongoDB\data"
net start MongoDB
```
但是我不可以这样写,会报错，这里的MongoDB，是默认名字吗？有待证实

搞明白了，MongoDB是默认名字，net start 也是windows独有的

关闭服务
```bash
mongo
use admin
db.shutdownServer()
```

发送post请求
```bash
curl -d "name=haimeimei&age=27" http://localhost:3000/users/addPerson
```
- -d post请求


读数据
```js
const result = await Person.findOne({
    name: ctx.request.body.name
  })
```
写数据要创建实例
```js
new Person({
  name: ctx.request.body.name,
  age: ctx.request.body.age
})
```

更新数据
```js
const result = await Person.where({
  name: ctx.request.body.name
  }).update({
    age: ctx.request.bdoy.age
  })
```
删除数据
```js
const result = await Person.where({
  name: ctx.request.body.name
  }).remove()
```
Redis
-----------
[安装教程](http://www.runoob.com/redis/redis-install.html)
服务端用session来保持用户状态，浏览器用cookie保存session，服务器把session种植到cookie中然后下次访问的时候cookie就会带着session过来，进而达到身份认证的过程

安装中间件
```bash
npm i koa-generic-session koa-redis
```
操作Redis方法
```bash
redis-cli

keys *
get key
```

Redis可以直接存储数据，不用配合session也行
```js
const Redis = require('koa-redis')

const Store = new Redis().client

router.get('/fix',async function(ctx) {
  const st = await Store.hset('fix', 'name', Math.random())
  ctx.body = {
    code: 0
  }
})

```
Redis 查看
```bash
keys *
hget fix name
```

Nuxt.js 是做vue ssr的一个框架
--------------
1. 是基于vue2这个框架做的
2. 包含了vue Router
  - vue本身不带路由，是通过插件的方式支持，Nuxt.js整合了这些
  - Nuxt.js将Router的配置设置的很简单，甚至你可以不配置就可以用
3. 可以支持vuex
  - vuex是一个跨组件的管理工具
4. 支持Vue Server Renderer
  - ssr,整合
5. 支持vue-meta
  - html meta管理


Nuxt生命周期

1. Incoming Request l浏览器发出请求
2. 检查有没有 nuxtServerInit 如果有的话就执行这个函数store action
3. middleware 中间件是跟路由相关，可以实现你想要的功能，都可以在这里完成
4. validate() 配合高级动态路由去做一些验证，比如说是不是允许跳到这个页面来，如果没有得到我的校验，就跳走之类的，
5. asyncData() & fetch() 获取数据
  - asyncData() 是用来获取渲染vue组件的
  - fetch() 通常是用来修改vuex的
6. Render 最后一步就是渲染

Navigate <nuxt-link> 如果要是有这个的话，会发起一个非(色沃)的路由他会重新循环3-6的内容，
如果<nuxt-link>发起的是一个新的路由，这个时候就从第一部开始循环1-6

Nuxt.js安装
[Nuxt.js安装](https://zh.nuxtjs.org/guide/installation)
```bash
vue init nuxt-community/koa-template .

```
今天研究了半天的bug，这玩意安装不成功呀
```
npm install jquery@latest 更新到最新版本

```
Nuxt.js官方安装，以前不支持koa，现在官方就可以自己支持
[Nuxt官网](https://zh.nuxtjs.org)

```bash
npx create-nuxt-app
```
安装的时候会有选项，可以选koa，不过这个koa是不支持import写法

#### 创建即配置，异步获取
创建及配置
vue里面是用mounted()方法发出ajax请求，但是这个不会在服务器端执行，只在浏览器端执行，
而asyncData()可以在服务端渲染，这样得到的数据更利于seo

Nuxtjs里面使用模板是要用到layout
```
export default {
  layout: 'search',
  data () {}
}
```

**SSR的作用，就是页面闪动的时候，ssr就可以做到**

环境准备与项目安装 6-2   6-1略过
-------------
用babel编译文件，这样就可以用import写法了
```bash
package.json 的dev后面增加babel编译
--exec babel-node

.babelrc 增加babel配置文件
{
  "presets": ["es2015"]
}

npm install babel-preset-es2015
```

支持sass编译
```bash
npm i sass-loader node-sass --save-dev
```
我也是醉了，安装sass后，`lang="scss"` 写成这样的，差点被坑了

nuxt.config.js
build,添加这个，增加缓存，不知道什么鬼
```bash
  cache: ture
```
这个东西在我的代码里，会导致element-ui 字体文件请求不到，会报错
暂时只能将这个代码去除了

首页开发
---------------------

真正做业务的时候要先想再做需求分析：

模板设计   复用

组件设计   如何拆分组件

数据结构设计  

接口设计

今天遇到了一个bug，就是@import引入css文件报错，最后就是我也不知道为什么引入header.scss这个文件就会报错，测试了一下午，原因就是写新代码没有创建新分支，所以导致了这种问题，不然直接切换分支，好像也不行，好像又可以，以后先要创建分支再写代码了，bug所在位置，components/public/header/topBar.vue: @import "@/assets/css/public/header.sass";
我再去测试一下是文件内容错误还是命名错误还是误认为是文件夹了，因为里面有个header文件夹

mdzz，后缀也是scss，而且header.scss内容也有问题，我TM也是醉了，搞我呢，一下午白忙

  
开发搜索模块
------------------------
搜索这个模块，原理特简单
判断是否获取焦点，是否有内容即可完成搜索框
return 是否焦点 && 是否有内容 //常规搜索
return 是否焦点 && !是否有内容  //热门搜索
还有要注意的是，没有焦点后搜索框就会瞬间消失，a链接就点不上了，所以要给个延迟
数据：
```js
  data () {
    return {
      isFocus: false,
      search: '',
      hotPlace: ['热门搜索', 'nice'],
      searchDate: ['哈哈2哈哈1', '也没2有那么1', '搜索2吗1', '北京1烤鸭', '偶是1'],
      searchList: [],
      show: true,
      number: 0,
      what: false,
      hidone: false
    }
  }
```
搜索框热门搜索出现逻辑代码:
```js
  computed: {
    isHotPlace () {
      return this.isFocus && !this.search
    },
    isSearchList () {
      return this.isFocus && this.search
    }
  },
  methods: {
    focus () {
      this.isFocus = true
    },
    blur () {
      setTimeout(() => {
        this.isFocus = false
      },200)
    },
```



搜索框代码：
```js
  watch: {
    search () {
      if (!this.search) {
        this.searchList = []
        return
      }
      if (this.what) {
        return
      }
      const result = []
      this.searchDate.forEach((value, index) => {
        if (value.indexOf(this.search) > -1) {
          result.push(value)
        }
      })
      /*和这层代码没关系*/
      for (let i = 0; i < this.searchList.length; i++) {
        this.$refs[i][0].className = ''
      }
      this.number = 0
      /*-------------*/
      this.searchList = result
    }

```

搜索框的down 和 up事件操控逻辑
写了很多冗余代码
```js
  numberSerace () {
      // 点击键盘down执行事件，写的很乱，不过功能还可以
      if (!this.search) return
      this.what = true
      this.hidone = true
      if (this.number >= this.searchList.length) {
        this.number = 0
      }
      for (let i = 0; i < this.searchList.length; i++) {
        this.$refs[i][0].className = ''
      }
      this.$refs[this.number][0].className = "msg"
      this.search = this.searchList[this.number]
      setTimeout(() => {
        this.what = false
      }, 200)

      this.number++
    },
    upSerace () {
      //点击键盘up执行事件
      if (!this.search) return

      this.what = true
      if (this.hidone) {
        this.number--
      }
      this.hidone = false
      this.number--
      if (this.number < 0) {
        this.number = this.searchList.length-1
      }
      for (let i = 0; i < this.searchList.length; i++) {
        this.$refs[i][0].className = ''
      }
      this.$refs[this.number][0].className = "msg"
      this.search = this.searchList[this.number]
      setTimeout(() => {
        this.what = false
      }, 200)
```
搜索框全代码地址：
[github地址](https://github.com/DemoorBug/vuesenior/blob/master/components/public/header/searchBar.vue)


7-8左侧边栏实现原理
---------------------
首先是结构布局
```html
<div>
  <dl>
    <dt>全部分类</dt>
    <dd><i>图标标签i</i>美食<span>>箭头标签</span></dd>
  </dl>
  <div> 这个div就是用来存放弹出内容的，是实时渲染，就是获得数据然后渲染，方便到爆
    <template> 这个标签控制循环，不然外面就要套一层，冗余
      <h4></h4>
      <span></span>
    </template>
  </div>
</div>
```
方法部分的实现原理分析：
- this.menu.filter((item) => item.type==this.kind)[0].child
  - 比对this.kind来获取想应数据，filter是什么？
- this.kind = e.target.querySelector('i').className
  - 获取当前鼠标对应的元素，赋值给this.kind
```html
  this._timer = setTimeout(() => {
    this.kind= ''
  }, 150)
这块代码延迟，可以在鼠标进入右侧边栏的时候有个反应时间，来让右侧响应标签取消这个定时器，从而达到可以防止鼠标到弹出框，弹出框就消失的问题
```
本节源码
```html
<template>
  <div class="m-menu">
    <dl
      class="nav"
      @mouseleave="mouseleave">
      <dt>全部分类</dt>
      <dd
        v-for="(item, index) of menu"
        :key="index"
        @mouseenter="enter">
        <i :class="item.type"/>{{ item.name }}<span class="arrow"/>
      </dd>
    </dl>
    <div
      v-if="kind"
      class="detail"
      @mouseleave="Eenter"
      @mouseenter="Emousele">
      <template v-for="(item, index) of curdetail">
        <h4 :key="index">{{ item.title }}</h4>
        <span
          v-for="v of item.child"
          :key="v">{{ v }}</span>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      kind: '',
      menu: [{
        type: 'food',
        name: '美食',
        child: [{
          title: '美食',
          child: ['代金券','饮品','火锅','自助餐']
        }]
      }, {
        type: 'takeout',
        name: '外卖',
        child: [{
          title: '外卖',
          child: ['美团','饿了么','百度外卖','自助餐']
        }]
      }]
    }
  },
  computed: {
    curdetail () {
      return this.menu.filter((item) => item.type==this.kind)[0].child
    }
  },
  methods: {
    mouseleave () {
      this._timer = setTimeout(() => {
        this.kind= ''
      }, 150)
    },
    enter (e) {
      console.log(e.target)
      this.kind = e.target.querySelector('i').className
    },
    Emousele () {
      clearTimeout(this._timer)
    },
    Eenter () {
      this.kind = ''
    }
  }
}
</script>
```