---
title: koa2 + vue2.5 + mongodb 开发记录
date: 2019-05-06 02:12:05
tags:
categories:
---

# 后端环境搭建
## node使用import语法 
@vue/cli 耦合，他会使用`babel.config.js`，而不会使用`.babelrc`，而且在`babel.config.js` 中配置会报错，所以要package.json 里面这样写
```js
script: {
  "start-koa": "nodemon --exec babel-node server/index.js --config-file ./.babelrc",
  "build-koa": "babel src --out-dir dist",
  "serve-koa": "node dist/server.js"
}
```
<!-- more -->
--config-file [path] 指定配置文件
要安装的包
```
@babel/cli
@babel/core
@babel/node          //新版本抽离出来了
@babel/preset-env    
nodemon
```

.babelrc
```js
{  
  "presets": ["@babel/preset-env"]
}
```

```bash

```