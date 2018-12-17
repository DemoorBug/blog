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
