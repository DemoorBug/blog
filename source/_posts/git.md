---
title: git
date: 2019-03-03 12:39:56
tags:
categories:
---
# 新电脑创建后快速使用git
```bash
cd ~ //切换到根目录
ssh-keygen -t rsa -C "demoorbug@gmail.com"
cd .ssh
cat id_rsa.pub // 将打印出来的复制到 github.com>settings>SSH and GPG Keys>New SSH
key
git config --global user.name "DEBUG"
git config --global user.email  "demoorbug@gmail.com"
```


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

# 撤销commit
这样就成功的撤销了你的commit
注意，仅仅是撤回commit操作，您写的代码仍然保留。
```bash
git reset --soft HEAD^
```
HEAD^的意思是上一个版本，也可以写成HEAD~1
如果你进行了2次commit，想都撤回，可以使用HEAD~2

options
--mixed 
意思是：不删除工作空间改动代码，撤销commit，并且撤销git add . 操作
这个为默认参数,git reset --mixed HEAD^ 和 git reset HEAD^ 效果是一样的。

--soft  
不删除工作空间改动代码，撤销commit，不撤销git add . 

--hard
删除工作空间改动代码，撤销commit，撤销git add . 

## 如果是注释写错了可以
```bash
git commit --amend // 就会进入编辑模式
```

# 拉取线上代码
```bash
git clone git@github.com:DemoorBug/pubg-api.git // 项目地址
```
