---
title: ptyhon
date: 2023-01-17 23:30:28
tags: [python]
categories: [python]
---

# ptyhon 的import
挺奇怪的东西，莫名其妙好了
z python3.9/site-pakages/myapplication.pth 这个目录是配置import的查找路径目录，可以通过如下命令查看是否添加成功，每一个路径通过换行来添加即可
```
python3
>>> import sys
>>> sys.path
```

什么东西啊，brew安装python-tk导致升级了python到3.10，然后安装pip install customtkinter 还是会找不到模块，把3.9的myapplication.pth复制到python3.10的相对目录下，才解决，用python3.9还是运行不了，还是提示缺少python-tk

- 如何引入呢？
比如全局目录有一个叫oauth2client的package，我们就可以通过
from oauth2client import tools 来引入该包的tools工具箱，不过vscode一直报错，莫名其妙，用终端尝试没问题，难道是要重启vscode？
这条命令在esmodule 中大概是这样的吧
import { run_flow } from 'oauth2client/tools'
使用命令就用这个
tools.run_flow() 虽然不知道python语法，但是通过ts猜想python中def大概是个声明语句


# pipenv
首先安装pyenv 版本控制python
brew install pyenv
用的fish，必须在config.fish下配置:
source (pyenv init - | psub)

安装pipx隔离pipenv环境
pip install -U --user pipx
pipx install pipenv

pipenv install customtkinter
但是执行pipenv run python main.py 会报错找不到模块customtkinter，
https://www.jetbrains.com/help/pycharm/pipenv.html#df546661 
大致如此：
python -m site --user-base # 输出的目录添加到环境变量即可
用这里的方法搞定了，不再报不知道模块，但是把这个环境变量删除之后，居然不会报错，也不知道是什么问题

但是还是会报No module named '_tkinter',这个只在mac环境下报，解决办法就是安装python-tk
brew install python-tk@3.9
安装完之后，直接执行命令还是报一样的错误
改为:
pipenv run python3.9 main.py 即可

好吧，原来没搞定啊，不报错是因为pip3.9 install customtkinter导致不报错的，卸载这个后原形毕露

终于搞懂了,找不到模块是因为我们用pipenv run python3.9(这里指定3.9)，但是我们的项目Pipfile里面的python是3.11，所以导致我们安装的包不在里面，把Pipfile里面的3.11改成3.9(为什么要3.9呢，因为我们python-tk用的是3.9的，如果用的3.11就不用改了)
改了后要执行pipenv --rm,pipenv install 重新创建虚拟环境，pipenv shell就可以进入环境，出现一个soruce ./path*** 回车即可。
在虚拟环境里面输入python 就是指定版本，import sys, sys.executable，是上面出现的目录就是成功进入了
然后pipenv run python main.py ，这样就是默认用的的python3.9版本，懂了



# pip删除所有包 
pip freeze>python_modules.txt
pip uninstall -r python_modules.txt -y

# 全局目录配置
python的全局配置目录(mac)在: $HOME/.config/pip/pip.ini

# brew 卸载及卸载依赖
brew uninstall *
brew autoremove
brew cleanup