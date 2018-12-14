---
title: vuesenior.md
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
git remote add origin git@github.com:DemoorBug@vuesenior.git
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

关闭服务
```bash
mongo
use admin
db.shutdownServer()
```