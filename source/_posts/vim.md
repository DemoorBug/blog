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

技巧48
ea = 在当前单词结尾后添加

w是指单词而W是指字串,有时候W可以更分辨,类似的还有W,B,E,gE

49:
t{char}及T{char}命令“知道查到指定字符为止”
通常, 当我想在当前行内快速移动光标时, 我倾向于在普通模式中使用f{char}和F{char}
命令; 当与d{mothion}或c{motion}一起使用时, 我会更倾向于使用t{char}及T{char}命令. 换句话说, 我在普通模式中会使用f和F, 而在操作服待决模式中则使用t和T.

var tpl = [
  '<a href="{url}">{title}</a>'
]