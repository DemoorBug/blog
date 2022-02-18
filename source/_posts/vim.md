---
title: vim
date: 2021-09-20 21:57:23
tags: vim
categories: vim
---

1. 一箭双雕:

   C == c$

   s  == cl

   S == ^C

   I == ^i

   A == $a

   o == A

   O == ko

2. 删除整个单词

   daw: 这个命令的好处是可以用`.`重复执行他 :h aw 可以解读为“delete a word”

3. 改变数字大小

   <C-a> and <C-x>

   10<C-a> and 10<C-x>

   > set nrformates=  该命令设置10进制

4. 操作命令

   c == 修改

   d == 删除

   y == 复制到寄存器

   g~ == 反转大小写

   gu == 转换为小写

   gU == 转换为大写

   \> ==增加缩紧

   < == 减小缩紧

   = == 自动缩紧

   ! == 使用外部程序过滤{motion}所跨越的行

5. 重绘屏幕达到写代码不滚至最底部的方法

   zz命令,太牛了,小技巧插入模式<C-o>zz

6. :normal命令

   结合@q来实现更复杂的功能

   qqfpRbaidu

   V50j

   :'<,'>normal @q

   or

   :%normal @q
