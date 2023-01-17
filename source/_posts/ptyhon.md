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

- 如何引入呢？
比如全局目录有一个叫oauth2client的package，我们就可以通过
from oauth2client import tools 来引入该包的tools工具箱，不过vscode一直报错，莫名其妙，用终端尝试没问题，难道是要重启vscode？
这条命令在esmodule 中大概是这样的吧
import { run_flow } from 'oauth2client/tools'
使用命令就用这个
tools.run_flow() 虽然不知道python语法，但是通过ts猜想python中def大概是个声明语句