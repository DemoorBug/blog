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