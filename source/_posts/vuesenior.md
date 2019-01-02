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

自己写的另一个列表源码
```html
<template>
  <section class="m-istyle">
    <dl @mouseover="moue">
      <dt>有格调</dt>
      <dd
        :class="{active:kind==='all'}"
        kind="all"
        keyword="景点">全部</dd>
      <dd
        :class="{active:kind==='part'}"
        kind="part"
        keyword="美食">约会聚餐</dd>
      <dd
        :class="{active:kind==='spa'}"
        kind="spa"
        keyword="丽人">丽人SPA</dd>
      <dd
        :class="{active:kind==='movie'}"
        kind="movie"
        keyword="电影">电影演出</dd>
      <dd
        :class="{active:kind==='travel'}"
        kind="travel"
        keyword="旅游">品质出游</dd>
    </dl>
    <ul class="ibody">
      <li
        v-for="(item, index) of curdetail"
        :key="index"
        style="height: auto">
        <el-card>
          <img :src="item.img">
          <ul class="cbody">
            <li class="title">{{ item.title }}</li>
            <li class="pos">
              <span
                v-for="(item, index) of item.pos"
                :key="index">{{ item }}</span>
            </li>
            <li class="price">￥<em>{{ item.price }}</em><span>/起</span></li>
          </ul>
        </el-card>
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  data () {
    return {
      kind: 'all',
      artistic: [{
        type: 'all',
        child: [{
          title: '木北造型（崇文门新世界店）',
          img: '//p1.meituan.net/wedding/8d26f93de654d433b17774e60a1fc5bd1028431.jpg@240w_180h_1e_1c_1l|watermark=1&&r=2&p=9&x=2&y=2&relative=1&o=20|368w_208h_1e_1c',
          pos: ['亲子场景酒店','发票推荐','亲子酒店点评'],
          price: '685'
        }, {
          title: '北京新侨诺富特饭店',
          img: '//p1.meituan.net/dnaimgdark/42487acaeaa40e3a97b41443f75566142443096.jpg@368w_208h_1e_1c',
          pos: ['亲子场景酒店','亲子酒店','房量充足'],
          price: '536'
        }]
      }, {
        type: 'part',
        child: [{
          title: '分米鸡 DM Chicken（合景·摩方店）',
          img: '//p1.meituan.net/msmerchant/a361a5683dcfc8db85d61f15639f9bfd61437.jpg@368w_208h_1e_1c',
          pos: ['分米鸡10DM套餐1份，有赠品'],
          price: '308'
        }, {
          title: 'COSTA COFFEE（东方新天地店）',
          img: '//p0.meituan.net/merchantpic/b5bc200ff96056aa58e2441c37b0097d58192.jpg@368w_208h_1e_1c',
          pos: ['巧克力酥卷小美式，建议单人使用'],
          price: '18'
        }]
      }, {
        type: 'spa',
        child: [{
          title: '魅贝克美发沙龙（崇文门新世界店）',
          img: '//p0.meituan.net/wedding/15570fc8c02870c37c21d4d250489cf9212844.jpg@240w_180h_1e_1c_1l|watermark=1&&r=2&p=9&x=2&y=2&relative=1&o=20|368w_208h_1e_1c',
          pos: ['魅贝克美发沙龙（崇文门新世界店）'],
          price: '282'
        }, {
          title: 'TONI&GUY（东方广场店）',
          img: '//p0.meituan.net/wedding/bdfdfa650660bc9c4b06c6a95f50492132697.jpg@240w_180h_1e_1c_1l|watermark=1&&r=2&p=9&x=2&y=2&relative=1&o=20|368w_208h_1e_1c',
          pos: ['TONI&GUY（东方广场店）'],
          price: '896'
        }]
      }, {
        type: 'movie',
        child: [{
          title: '金泉港IMAX国际影城',
          img: '//p0.meituan.net/poi/c2b482474377fc31a8311f46055f40b0442616.png@368w_208h_1e_1c',
          pos: ['免押金,可停车,有情侣座,儿童票,有WIFI,IMAX厅,杜比全景声厅,4DX厅'],
          price: '43'
        }, {
          title: '中影国际影城(丰台千禧街店)',
          img: '//p1.meituan.net/deal/__29661209__6853522.jpg@368w_208h_1e_1c',
          pos: ['免押金,可停车,有情侣座,儿童票,可刷卡,休息区,中国巨幕厅,60帧厅'],
          price: '19.9'
        }]
      }, {
        type: 'travel',
        child: [{
          title: '新鲜酒店',
          img: '//p1.meituan.net/tdchotel/3a2020cce42f452cb089eb274b67d6d18153330.jpg@368w_208h_1e_1c',
          pos: ['新客超值优惠','酒店套餐','亲子酒店'],
          price: '358'
        }, {
          title: '北京商旅智选北京INN公寓',
          img: '//p1.meituan.net/tdchotel/7d2e528c129d1376224df99dac1d3b382809929.jpg@368w_208h_1e_1c',
          pos: ['新客超值优惠','酒店套餐','亲子酒店'],
          price: '404'
        }]
      }]
    }
  },
  computed: {
    curdetail () {
      return this.artistic.filter((item) => item.type==this.kind)[0].child
    }
  },
  methods: {
    moue (e) {
      let dom = e.target
      let tag = dom.tagName.toLowerCase()
      if (tag == 'dd') {
        this.kind = dom.getAttribute('kind')
      }
    }
  }
}
</script>
<style lang="scss">
    @import "@/assets/css/index/artistic.scss";
</style>


```
8-1注册
-----------------------------
这章就是介绍接口的设计，很重要把，对于后台来说




|user表|areas表|menu表|city表|
|--|--|--|--|
|username||||
|pasword||||
|email||||

接口
/users/signup
/users/signin
/users/verify
/users/exit
/users/getUser


这一章涵盖太多不知道的东西了，必须要搞明白，为什么这么写，实现的是什么功能，再开始继续，把每一个代码都记录下来，不管是百度还是什么


nodemailer  控制邮箱的一个应用
npm install nodemailer

[axios教程](https://blog.csdn.net/qq_36801710/article/details/79528013)
创建一个axios实例
axios.create({
  baseURL:
})

passport 权限认证中间件，本地用户登陆，第三方登陆
[passport教程](https://segmentfault.com/a/1190000011557953)


这个老师写了一大堆错误，简直逻辑不同，根本无法用，然后老师一笔带过，简直就是误人子弟
我自己解决了所有遇到的问题
这节可以去看代码的编写，很简单
接口代码实现笔记：
[users.js本节项目地址](https://github.com/DemoorBug/vuesenior/tree/master/server/interface/users.js)
```html
import Router from 'koa-router'                 //路由
import Redis from 'koa-redis'                   //redis数据库，主要是提交验证码的时候写入验证码数据
import nodeMailer from 'nodemailer'             //发邮件的app
import User from '../dbs/models/users.js'       //mongoose
import Passport from './utils/passport.js'      //自动登陆？
import Email from '../dbs/config.js'            //配置文件
import axios from './utils/axios.js'            //跨平台请求

let router = new Router({                       //这个页面放到/users下
  prefix: '/users'
})

let Store = new Redis().client

router.post('/signup', async (ctx) => {  //注册
  const {
    username,
    password,
    email,
    code
  } = ctx.request.body    //解构赋值

  if (code) {    //判断是否输入code
    const saveCode = await Store.hget(`nodeMailer:${username}`,'code')
    const saveExpire = await Store.hget(`nodeMailer:${username}`,'expire')
    if (code === saveCode) { //判断是否和redis数据库的code生成的验证码一致
      if (new Date().getTime-saveExpire>0) { //判断是否和redis数据库时间过期
        ctx.body = {
          code: -1,
          msg: '验证码过期，请重新尝试'
        }
        return false
      }
    } else {   //saveCode 错误执行
      ctx.body = {
        code: -1,
        msg: '请填写正确的验证码 '
      }
      return false
    }
  } else {  //不存在code
    ctx.body= {
      code: -1,
      msg: '请填写验证码'
    }
    return false
  }

  let user = await User.find({  //查询mongodb数据库
    username
  })

  if (user.length) {  //如果有用户，提示报错
    ctx.body = {
      code: -1,
      msg: '已被注册'
    }
    return
  }

  let nuser = await User.create({   //写入mongodb数据库，这里是创建一个实例
    username,
    password,
    email
  })
  if (nuser) {  //写入成功执行
    let res = await axios.post('/users/signin', {username, password})
    //自动登陆？大概吧
    if (res.data && res.data.code === 0) { 
      ctx.body = {
        code: 0,
        msg: '注册成功',
        user: res.data.user
      }
    } else {
      ctx.body = {
        code: -1,
        msg: 'error'
      }
    }
  } else {
    ctx.body = {
      code: -1,
      msg: '注册失败'
    }
  }
})

router.post('/signin', async (ctx, next) => {  //自动登陆？
  return Passport.authenticate('local', function(err, user, info, status) {
    if (err) {
      ctx.body = {
        code: -1,
        msg: err
      }
    } else {
      if (user) {
        ctx.body = {
          code: 0,
          msg: '登录成功',
          user
        }
        return ctx.login(user)
      } else {
        ctx.body = {
          code: 1,
          msg: info
        }
      }
    }
  })(ctx, next)
})

router.post('/verify', async (ctx, next) => { //验证码接口
  let username = ctx.request.body.username   //用户名
  const saveExpire = await Store.hget(`nodeMailer:${username}`,'expire')
  if (saveExpire && new Date().getTime-saveExpire<0) {
    ctx.body = {
      code: -1,
      msg: '验证请求过于频繁，1分钟一次'
    }
    return false
  }
  let transporter = nodeMailer.createTransport({
    host: Email.smtp.host,
    port: 587,
    secure: false,
    auth: {
      user: Email.smtp.user,
      pass: Email.smtp.pass
    }
  })
  let ko = {
    code: Email.smtp.code(),
    expire: Email.smtp.expire(),
    email: ctx.request.body.email,
    user: ctx.request.body.username
  }
  let mailOptions = {
    from: `"您的邮件"<${Email.smtp.user}>`,
    to: ko.email,
    subject: '《demoorbug》注册码',
    html: `您的验证码是${ko.code}`
  }
  await transporter.sendMail(mailOptions, (error, info) => {
    if (error) {
      return console.log('error')
    } else { //写入redis数据库
      Store.hmset(`nodeMailer:${ko.user}`, 'code', ko.code, 'expire', ko.expire, 'email', ko.email)
    }
  })
  ctx.body = {
    code: 0,
    msg: '验证码已发送，可能会有延迟，有效期1分钟'
  }
})

router.get('/exit', async (ctx, next) => {
  await ctx.logout()
  if (!ctx.isAuthenticated()) {
    code: 0
  } else {
    ctx: -1
  }
})

router.get('/getUser', async (ctx) => {
  if(ctx.isAuthenticated()) {
    const {username, email} = ctx.session.passport.user
    ctx.body = {
      user: username,
      email
    }
  } else {
    ctx.body = {
      user: '',
      email: ''
    }
  }
})

export default router


```
目前就学习到/verify接口，前面的接口bug都已经解决，这段还没有解决，因为我感觉是有问题的代码，后面可以深入一下这个地方，多注意下，有问题就回来解决:


register.vue 的验证码函数和提交函数的编写
[register.vue文件地址](https://github.com/DemoorBug/vuesenior/tree/master/pages/register.vue)
```html
  methods: {  //验证码函数
    sendMsg () {  
      let namePass  
      let emailPass
      if(this.timerid) {
        return false
      }
      this.$refs['ruleForm'].validateField('name', (valid) => { //validateField这是饿了么组件自带的方法，应该是。查询name是否有值，没有值，则valid返回提示的值，比如name不输入提示"请输入昵称"
        namePass = valid
      })
      this.statusMsg = '' //验证码的双向绑定数据
      if(namePass) {  //
        return false
      }
      this.$refs['ruleForm'].validateField('email', (valid) => { //和上面同理
        emailPass = valid
      })  
      if (!namePass && !emailPass) { //如果都填写了，则不会赋值就为空所以是false,要取反
        this.$axios.post('/users/verify', { //可以直接用$axios是因为nuxtjs内置了
          username: encodeURIComponent(this.ruleForm.name),  用户名转换为字符串
          email: this.ruleForm.email 
        }).then(({status, data}) => {  //回调函数，解构赋值
          if (status === 200 && data && data.code === 0) {
            let count = 60;   
            console.log('1')
            this.statusMsg = `验证码已发送,剩余${count--}秒`
            this.timerid = setInterval(() => {  //定时器刷新
              this.statusMsg = `验证码已发送,剩余${count--}秒`
              if (count === 0) {
                this.statusMsg = ''
                clearInterval(this.timerid)
              }
            }, 1000)
          } else {
            this.statusMsg = data.msg
          }
        })
      }
    },
    register () {
      this.$refs['ruleForm'].validate((valid) => {  //饿了么组件自带的方法,全部验证通过valid = true
        if (valid) {
          this.$axios.post('/users/signup', {
            username: window.encodeURIComponent(this.ruleForm.name),
            password: CryptoJs.MD5(this.ruleForm.pwd).toString(),
            email: this.ruleForm.email,
            code: this.ruleForm.code
          }).then(({status, data}) => {
            if (status ===200) {
              if (data && data.code ===0) {
                // this.error = data.msg
                location.href = '/login'
              } else {
              this.error = data.msg
              }
            } else {
              this.error = `服务器出错，错误码: ${status}`
            }
            setTimeout(() => {
              this.error = ''
            }, 1500)
          })
        }
      })
    }
  }

```

index 的一些修改备注
[index.js地址](https://github.com/DemoorBug/vuesenior/blob/master/server/index.js)

```html
import mongoose from 'mongoose'              //mongoose
import bodyParser from 'koa-bodyparser'      //unkown
import session from 'koa-generic-session'    //session
import Redis from 'koa-redis'                //redis
import json from 'koa-json'                  //格式化json
import dbConfig from './dbs/config'
import passport from './interface/utils/passport.js'  
import users from './interface/users'

app.keys= ['debug', 'keyskeys']    //unkown
app.proxy = true                   //unkown
app.use(session({                  //session
  key: 'debug',
  prefix: 'debug:uid',
  store: new Redis()
}))
app.use(bodyParser({                
  extendTypes: ['json', 'form', 'text']
}))
app.use(json())             //格式化diamagnetic

mongoose.connect(dbConfig.dbs, {    //mongodb地址？应该是
  useNewUrlParser: true
})

app.use(passport.initialize())       //unkown
app.use(passport.session())          //unkown



app.use(users.routes()).use(users.allowedMethods())  //将我们写好的路由引入
```

## 接口设计(需求分析)
/geo/getPosition
/geo/province
/geo/province/:id
/geo/city
/geo/hotCity     /geo/menu

查询接口
/search/top
/search/resultsByKeywords
/search/hotPlace
/search/products
/search/product/:id

`mongoimport -d dbs -c test pois.dat`
dbs对应database
test对应Collections
pois.dat 对应数据源

签名：没有签名获取不到数据
http://cp-tools.cn/sign
c6a3d36c8d43371e21550e1420f0d19e
https://www.imooc.com/u/3164558/bbs
从这个人获取到了UID。。

怎么说呢，还行吧，不过是用wsl(windows的子系统ubuntu)提交会出问题，有些东西提交了两遍

```js
import Vue from 'vue'
import Vuex from 'vuex'
import geo from './modules/geo'
import home from './modules/home'

Vue.use(Vuex)

const store = () => new Vuex.Store({
  modules: {
    geo,
    home
  },
  actions: {
    async nuxtServerInit({commit}, {req, app}) {
      //nuxtServerInit这个是nuxtjs提供的生命周期钩子
      const {status, data: {province, city}} = await app.$axios.get('/geo/getPosition')
      commit('geo/setPosition', status ===200 ? {city, province}: {city: '', province: ''})
      const {status: status2, data: {menu}} = await app.$axios.get('/geo/menu')
      commit('home/setMenu', status2 === 200 ? menu : [])
    }
  }
})

export default store
```

放弃，只是带我抄了一遍代码，基本没什么学的，垃圾