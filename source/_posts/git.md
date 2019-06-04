---
title: git
date: 2019-03-03 12:39:56
tags:
categories:
---

# 创建项目
```bash
mkdir shop //创建shop文件夹
cd shop
git init //初始化项目
touch README.md //创建初始化项目文件
git add .
git commit -m "shop" //本次提交信息简介
git remote add origin git@github.com:DemoorBug/shop.git
git push -u origin master //-u代表了关联本地分支和线上分支的master分支，以后就可以直接git push了
```
查看remote以及删除重新添加方法
```bash
git remote -v  
git remote remove <name>
```
# 分支
```bash
git checkout -b index-redux //创建分支
git push origin index-redux //推送到线上，没有就创建
git checkout master
git merge origin/index-redux // 合并分支
git push
```
查看分支
```bash
git branch -a // 或
git branch
```

删除分支
```bash
git branch -d index-redux // 删除本地分支
git push origin --delect index-redux // 删除远程分支
```
