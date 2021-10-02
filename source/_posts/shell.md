---
title: shell
date: 2018-09-14 23:55:43
tags: [bash,linux]
categories: [github项目笔记]
---

# linux命令
[linux命令](https://github.com/DemoorBug/Shell)

<!-- more -->
# 所学命令的一些大致顺序 以及简介
|命令|常用参数|命令英文原意|简介|
|:--:|:--:|:--:|:--|
|2016-9-22| | | |
|pwd|no|print working directory|查询所在目录位置|
|ls|-aldhi||查询目录内容|
|-rw-r--r--||no|r读w写x执行|
|mkdir|-p|make directories|建立目录|
|cd|~ - .. .|change directory|切换所在目录|
|tab|no||目录补全,命令补全,按两下就会显示当前目录的东西|
|rmdir|no|remove empty directories|删除，但是不常用，有文件内容就无法删除|
|rm|-rf|remove|删除文件或目录|
|cp|-rpd -a|copy|复制命令-a相当于-rpd|
|mv||move|剪切或改名命令|
|2016-9-23| | | |
|ln|-s|link|链接命令 **最好使用-s(软链接)方式创建**|
|locate|||文件搜索命令，搜索速度快,在/var/lib/mlocate 下搜索后台数据库|
|updatedb|||每天更新一次，所以可以手动更新|
|whereis|-bm||搜索命令的命令,ls.1.gz 1 命令 man -f ls ，man 1 ls 一致|
|which|||搜索命令的命令。搜索命令所在路径及别名|
|find|||-iname -user -nouser **find / -name install.log**|
|* ? []|||通配符*任意内容?一个内容[]任意一个中括号中的内容|
|-exec {} \ |||可以在其他命令之后再继续执行命令|
|grep|-iv||搜索字符串命令 grep [选项] 字符串 文件名|
|man|-f||帮助命令 man ls **man 需要下载包** 编辑模式\-d 然后按n就可以向下跳 shift上|
|passwd|||找到所有关于|
|ls --help|||在xShell中显示中文|
|help|||获取内部命令 介绍|
|info|||帮助查询，回车进入(带有*号) u 进入上层页面 n进入下一节 p进入上一节 q退出|
|zip|-r||zip 压缩文件名 源文件  zip -r 压缩文件名 源目录|
|unzip|||解压缩|
|touch|-acfm||用来更新文件的或目录的时间，文件不存在则创建文件|
|gzip|||.gz 格式压缩 gzip 源文件 #这样源文件会消失，gzip -c 源文件 > 压缩文件源文件保留,gzip -r 目录 压缩目录下的所有子文件，不能压缩目录|
|ls > abc|||将ls 当前数据写到 abc文件下|
|cat|||输出文件|
|gunzip gzip -d|||两种都可以解压缩|
|bzip2|||bzip2 -k 源文件 这样可以保留源文件，bzip2 源文件 `不能压缩目录`|
|bzip2 -d bunzip2|||解压缩|
|tar|-cvf||打包命令，例如 tar -cvf longzls.tar longzls `打包后压缩`|
|tar|-xvf||解打包命令 -x解打包|
|tar|-zcvf||直接将其打包解压gzip|
|tar|-jcvf||直接将其打包为bzip2|
|tar|-ztvf||t代表测试的意思，只查看里面内容但是不解压|
|shutdown|-chr||shutdown [选项] 时间|
|date||时间|
|&|||shutdown -r 05:30 & 代表后台执行 ，吧这条命令了放到后台组不占用当前操作字段|
|runlevel|||查看运行级别|
|**logout**|||退出登录命令|
|mount|-a||挂载命令|
|mount|-to||mount \[-t文件系统] [-o特殊选项] 设备文件名 挂载点|
|umount|||卸载命令|
|fdisk|-l||查看已经读取的设备|
|w|||查看登录用户信息|
|who|||查看登录用户信息|
|last|||查询当前登录和过去登录的用户信息，/var/log/wtmp/这个文件是被编译的，一旦修改直接崩溃|
|lastlog|||查看所有用户的最后一次登录时间，/var/log/lastlog/|
|echo $SHELL|||打印当前环境的shell用的是什么sh、ksh、Bash、psh、zsh|
|echo|-e||输出命令,echo \[选项] \[输出内容]|
|vim|||貌似是创建一个文本|
|chmod 755 hello.sh|||赋予执行权限|
|bash hello.sh|||通过Bash调用执行脚本|
|alias|||显示已有的别名,alias ls="ls --color=never" 定义别名|
|history|-cw||历史命令 history \[选项] \[历史命令保存文件]|
|wc|-cwl||输入重定向，wc \[选项]\[文件名]|
|more|||分屏显示|
|netstat|-an|||
|df|-lahHTtx||磁盘管理|
|du|-bkmhHs||统计磁盘文件大小|
|fdisk|||分区|
|passwd|||修改密码|
|source|||配置文件 或 . 配置文件|
|||||
|||||
> locate 遵循/etc/updatedb.conf 配置文件

> 常见压缩格式 ： .zip .gz .bz2 .tar.gz .tar.bz2

> tar -jxvf jp.tar.bz2 -C /tmp/ 指定目录解压缩

> tar -jxvf /tmp/jp.tar.bz2 jp anaconda  压缩多个文件，并且指定目录

> shutdown -r now 立即重启   -h关机 -c取消前一个关机命令

> cat /etc/inittab  决定你启动是在字符界面还是图像界面 **默认运行级别**

> linux 中大写X代表图形界面

> mount -t vfat /dev/sdb1 /mnt/usb/  挂载U盘 vfat = fat32

> 光盘设备文件名 /dev/sr0  

> mount -t iso9660 /dev/sr0 /mnt/cdrom/ 挂载光盘

> 按i, I, o, O, a, A, r, R任一字符进入输入模式  

##环境变量
````
用户自定义变量(本地变量)
环境变量    环境变量作用:定义每个用户的操作环境。已学的环境变量：path
预定义变量
位置参数变量

source 配置文件
或
. 配置文件
修改配置文件后，必须注销重新登录才能生效
使用source命令可以不用重新登录
/etc/profile
/etc/profile.d/*.sh
~/.bash_profile
~/.bashrc
/etc/bashrc


本地终端欢迎信息：/etc/issue
\l  显示登陆终端号，共6个
远程登陆欢迎信息：/etc/issue.net
显示欢迎信息，由ssh的配置文件/etc/ssh/sshd_config决定，加入
"Banner /etc/issue.net" 行才能显示(重启ssh服务)
````

##用户和用户组
```
sudo adduser username 创建用户
/etc/group 存储当前系统中所有用户组信息
组名称:组密码占位符:组编号:组中用户名列表
/etc/gshadow 存储当前系统中用户组的密码信息
组名称：组密码：租管理者：组中用户列表 * ！可以认为密码为空
/etc/passwd 存储当前系统中所有用户的信息
用户名:密码占位符:用户编号：用户编号组：用户注释信息：用户主目录：shell类型
/etc/shadow 存储当前系统中所有用户的密码信息

groupadd sexy
groupmod -n market sexy 修改组名
groupmod -g 668 market 修改组编号
groupadd -g 888 boss 创建box 直接改变组编号
groupadel market 删除 用户组 必须先删除用户

用户
groupadd sexy
useradd -g sexy sdf   -g指定用户组
useradd -g sexy jzmb
useradd -d /home/xxx imooc -d指定用户文件夹  没有写就会直接创建同名文件夹
usermod -c dgdzmx sdf  修改sdf用户备注
usermod -l cls sdf  新用户写到前面，旧用户写到后面 修改
usermod -d /home/cls cls 修改cls用户文件夹
usermod -g sexy imooc 将用户imooc切换到 sexy
userdel jzmb 删除用户jzmb 不会删除文件
userdel -r jzmb 删除他的个人文件


touch /etc/nologin  出了root 用户 其他人都无法登陆   在~目录创建就好

passwd -l cls 锁定账户
pwsswd -u cls 解锁
passwd -d cls 清除密码
gpasswd -a cls boss 附属组
gpasswd -a cls boss,sexy 多个附属组
newgrp boss 切换附属组 要输入组密码
gpasswd =d cls boss 清除附属组

useradd -g group1 -G group2,group3,········ 及指定主要组和附属组
gpasswrd imooc  设置组密码


su username   切换用户
Whoami 我谁谁？显示当前登录用户名
id cls  显示指定用户信息，包括用户编号，用户名
groups cls 显示cls用户所在组
chfn cls 设置用户资料，一次输入用户资料
finger imooc 显示用户详细资料
```


##swap
```
fdisk /dev/sdb
p
t
磁盘1-n
L 编号

mkswap /dev/sdb5  格式化
swapon /dev/sbb5 启用
free 加载状况
swapoff /dev/sdb5 停止
```

##自动挂载
```
vim + /etc/fstab
/dev/sdb1 /mnt/imooc ext4 defaults 0 0
```

##格式化

```
mkfs.ext4 /dev/sdb1
mkfs -t ext4 /dev/sdb2
```

##分区
```
fdisk 查看帮助
fdisk -l  最后一块就是我们新添加的盘
fdisk /dev/sdb
m  查看帮助信息
n 添加分区   p表示主分区  e表示扩展分区
p 查看当前分区表

d 删除分区
w 写入方案 保存

parted 分区 MBR GPT 两个都可以使用此工具
help 帮助信息
select /dev/sdc
mklabel gpt
print 查看当前分区详情
print all 查看所有
mkpart 添加分区 提问模式 交互模式
mkpart test 2000 3000 命令模式
rm 3 删除第三块分区
unit GB 位单位
```

## VIM
```
vim + abc 直接进入最后一行
vim +2 abc 进入第二行
vim +/a abc 查看所有带A的高亮  命令模式按n切换
vim aa bb cc 打开多个文件，n下一页 N 上一页
底行模式常用指令
:w  保存修改
:q  退出
:!  强制执行
:ls 列出当前打开的所有文件
:n N
:15  快速定位
/xxx 从光标位置向后搜索
?xxx 从光标位置向前搜索
命令模式
h 光标左移
j 光标下移
k 光标上移
l 光标右移
ctrl+f 向下翻页
ctrl+b 向上翻页
ctrl+d 向下翻半页
ctrl+u 向上翻半页
dd 删除光标所在行
o 在光标所在行的下方插入一行并切换到输入模式
yy 复制光标所在的行
p 在光标所在行的下方粘贴
P 在光标的所在行的上放粘贴
```

## 通配符
```
？ 一个字符
*   所有
[]  [abc]
[-] [0-9][0-9]
[^] [^0-9]逻辑非
----------
`` 是用来包含系统命令的 $(ls) 替代方案
'' 变量 \ $ 不会被执行
"" 和单元号相反
# 代表注释
$ 用于调用变量，调用name的时候，需要$name方式
\ 转以符合
```

##管道符
```
1 多命令顺序执行
;   命令1;命令2
&&  命令1&&命令2
||  命令1||命令2
2.管道符
这个才是管道符
命令1 | 命令2  用命令2去操作命令1打印出来的东西

netstat -an | grep ESTABLISHED | wc -l 通过他我们可以知道服务器连接多少人
```

## 重定向
```
ls > test.log   文件不存在创建并修改
ls >> test.log  追加
detecang 2>>test.log 错误信息追加，没有空格

ls >> test.log 2>&1
ls &>> test.log 不管错误还是标准 都追加
ls $> /dev/null 丢进黑洞

ls>>文件1 2>>文件2 正确的保存到文件1 错误的保存到文件2   
```

##历史命令的调用
```
!n 重复执行第N条历史命令
!! 重复执行上一条命令
!字符 重复执行最后一条以该字符开头的命令
```

## 快捷键
```
ctrl+c 强制终止当前命令
ctrl+l 清屏   clear
ctrl+a 光标移动到命令行首
ctrl+e 光标移动到命令行尾
ctrl+u 从光标所在位置删除到行首
ctrl+z 把命令放入后台 &
ctrl+r 在历史命令中搜索
```

## 环境变量
```
vi ~/.bashrc
#写入环境变量的配置文件
source .bashrc 直接生效，或者重新登录
unalias cp 临时删除
```

## 第一个脚本
```
vi hello.sh
#!/bin/bash  必须有
#The first program 注释

echo -e "\e[1;34m 天上掉下个林妹妹 \e[0m"
```

## shell
```
vi /etc/shells  可以查看当前Bash与sh兼容
sh 进入 sh shell
exit 退出
csh 进入 csh shell

```


##mount -o指定一些操作
```
atime/noatime   更新访问时间/不更新访问时间。访问分区文件时,是否更新文件的访问时间,默认更新
async/sync  异步/同步，默认为异步
auto/noauto 自动/手动,mount -a 命令执行时,是否会自动安装/etc/fstab文件内容挂载，默认为自动
defaults    自动默认值,相当于re,suid,dev,exec,auto,nouser,async这七个选项
exec/noexec  执行/不执行,设定是否允许在文件系统中执行科执行文件,默认是exec允许
remount 重新挂载已经挂载的文件系统,一般用于指定修改特殊权限
rw/ro   读写/只读,文件系统挂载时,是否具有读写权限,默认是rw
suid/nosuid 具有/不具有SUID权限,设定文件系统是否具有SUID和SGID的权限，默认是具有
user/nouser 允许/不允许普通用户挂载,设定文件系统是否允许普通用户挂载,默认是不允许,只有root可以挂载分区
usrquota 写入代表文件系统支持用户磁盘配额,默认不支持
grpquota 写入代表文件系统支持组磁盘配额,默认不支持

```

## find ，-exec
```


> find /var/log/ -mtime +10 查找10天前修改的文件
> -10 10天内修改的文件
> 10 10天当天修改的文件
> +10 10天前修改的文件
>
>  atime 文件访问时间
>  ctime 改变文件属性
>  mtime 修改文件内容
>  
> find . -size 25k 查找文件大小是25K的文件
> **25M M要大写**
> -25k 小于25KB的文件
> 25K 等于25KB的文件
> + 大于 25KB的文件
>
> find . -inum 262422
> 查找I节点是262422的文件
>
> find /etc -size +20k -a -size -50k
> #查找/etc目录下，大于20KB并且小于50KB的文件
> -a and 逻辑于
> -o or 逻辑或
>
> find /etc -size +20k -a -size -50k -exec ls -lh {} \;
```

## 常用目录的作用
```
/根目录
/bin命令保存目录 (普通用户就可使用) /sbin (root才可修改) /usr还有bin目录意思同前面
/boot 启动目录 保存的是启动数据
/dev 特殊文件目录
/etc 保存的是默认配置文件
/home 是普通用户目录
/root 超级管理员目录
/lib 函数库
/mis /media /mnt 挂载U盘 空目录，所有存储设备都要挂着后使用，挂载就是分配盘符，用着作为外接存储的盘符 /mnt挂U盘 移动硬盘，老师在/mnt目录下创建CDrm 挂载光盘，同时创建mst挂载U盘
*** 为什么呢 因为老式的linux 没有其他两个目录 /mis挂载磁带机 /media 挂载光盘  ***

/proc /sys 不能直接操作，保存的是内存的挂载点，直接写在内存当中，无法写入数据
/tmp 临时目录
/usr 系统资源保存目录
/var 系统相关文档
*** 练习的话就在tmp和root，home下 ***

```

ps aux 查看运行程序

## scp 的使用
-v 和大多数 linux命令中的-v意思一样，用来显示进度。可以用来查看连接、认证、或是配置错误

-C 使能压缩选项

-P 选择端口

-r 复制目录



1、从本地将文件传输到服务器
scp【本地文件的路径】【服务器用户名】@【服务器地址】：【服务器上存放文件的路径】



scp /Users/mac_pc/Desktop/test.png root@192.168.1.1:/root



2、从本地将文件夹传输到服务器
scp -r【本地文件的路径】【服务器用户名】@【服务器地址】：【服务器上存放文件的路径】



sup -r /Users/mac_pc/Desktop/test root@192.168.1.1:/root



3、将服务器上的文件传输到本地
scp 【服务器用户名】@【服务器地址】：【服务器上存放文件的路径】【本地文件的路径】



scp root@192.168.1.1:/data/wwwroot/default/111.png /Users/mac_pc/Desktop



4、将服务器上的文件夹传输到本地
scp -r 【服务器用户名】@【服务器地址】：【服务器上存放文件的路径】【本地文件的路径】



sup -r root@192.168.1.1:/data/wwwroot/default/test /Users/mac_pc/Desktop


---------------------
作者：小猪熊121
来源：CSDN
原文：https://blog.csdn.net/aa294194253/article/details/50054723
版权声明：本文为博主原创文章，转载请附上博文链接！

## 查看进程，杀死进程
```bash
ps -aux // 查看进程
kill -s 9 PID // 上面的命令可以查出PID

```
## sudo npm sudo node 报错找不到命令
```bash
//原因很简单，没有放到/usr/local/bin目录下，所以我们创建软链接即可
while node //查看命令所在路径
while npm
sudo ln -s /home/demoorbug/.nvm/versions/node/v14.5.0/bin/node /usr/local/bin/
sudo ln -s /home/demoorbug/.nvm/versions/node/v14.5.0/bin/npm /usr/local/bin/
//done
```

# brew 安装

```bash
curl -L https://github.com/Homebrew/brew/tarball/master | tar xz --strip 1 -C ~/homebrew
# 环境变量配置
export PATH="$HOME/.linuxbrew/bin:$PATH"
export LD_LIBRARY_PATH="$HOME/.linuxbrew/lib:$LD_LIBRARY_PATH"
source ~/.bashrc
```

# z 配置

```bash
brew install z
brew list z
vim ~/.bashrc
. /home/demoorbug/homebrew/Cellar/z/1.9/etc/profile.d/z.sh #这条命令粘贴到.bashrc
source ~/.bashrc
```



```tsx

```
# 技巧
[https://www.youtube.com/watch?v=gxBis8EgoAg&t=1010s](https://www.youtube.com/watch?v=gxBis8EgoAg&t=1010s)
```bash
find packages -type f | grep vue //查找packages下所有带vue带文件
find packages -type f | grep vue | xargs rm //删除所有筛选到到文件
```


