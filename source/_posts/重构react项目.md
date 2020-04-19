---
title: pubg-react
date: 2020-03-04 20:50:42
tags: [react]
categories: [学习]
---

看到别人的一个项目不错，而且开源，react的，很多不懂，现在看能不能重构成功吧
项目名pubg-react
<!-- more -->
```bash
npx create-react-app pubg-react
```



# client用到的库，介绍
1. apollo-client

# server端
## 开始
用的最新hapi, 安装命令 `yarn add @hapi/hapi`
这里需要注意，新版必须使用node 12.x 以上的版本，否则就会报错
```bash
sudo npm install n -g

sudo n stable

PATH="$PATH" // 更新环境变量，不然还是旧版
```
要在node端使用es6语法，用babel实现，安装`yarn add @babel/preset-env @babel/node @babel/core -D`
创建.babelrc 文件：
```json
{
  "presets": ["@babel/preset-env"]
}
```
开发环境安装nodemon `yarn add nodemon -D`
package.json:
```json
{
  "scripts": {
    "dev": "nodemon -w src --exec babel-node src/app.js"
  }
}
// shell 里面使用babel-node 的话，要用npx babel-node
// 注意: 先要安装@babel/node @babel/core ，不然babel-node使用的就行旧版的babel-node 6.x
如果是生产环境，就不要使用babel-node
```
[https://github.com/babel/example-node-server 生产环境配置](https://github.com/babel/example-node-server)


## 用到的库
Hapi.js 如果仅是用于返回 api接口，或者通过node调用其他网络接口
socket.io 封装了HTML5的websocket，完成一次握手，就可以畅通通讯
dotenv 可以轻松使用process.env. 环境变量配置项目，只要在项目目录创建.env 文件
pgr 是数据库连接的工具Postgres DB, 这原来是一个作者开发的啊。
