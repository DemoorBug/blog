---
title: Node+mongodb+nginx+linux
date: 2018-09-11 19:42:40
tags: [node,mongodb,nginx,linux,ubuntu]
categories: [github项目笔记,node]
---

## node项目的部署
项目地址：[Node+mongodb+nginx+linux](https://github.com/DemoorBug/Node-mongodb-nginx-linux)
**本次服务器为centos，只是临时在虚拟机建立的，虚拟机密码为1004041672**
**696px网站的部署以及linux一些命令记录，还有nginx是怎么用的，到底是个什么东西**
<!-- more -->


## 4-1~2
## 通过ssh实现无密码登陆
**客户机**
```bash
pwd
cd /user/demoorbug | cd ~
找到是否有.ssh这个文件夹如果有id_rsa id_rsa.pub(公钥)就是说明配置过,如果需要进行下面的的步骤的话最好改名再继续
ssh-key -t rsa -b 4096 -C "465298643@qq.com"
虽然不知道为什么要敲这个命令
接下来是把这个ssh代理开起来
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa 加入代理
最终目的呢就是拿到id_rsa
```

**进入linux终端**
```c
输入和windows一样的命令
要把windows上的公钥内容粘贴到 linux的authorized_keys下
但是还是要有几个动作来完成，首先是授权
chmod 600 authorized_keys
sudo service ssh restart 
```


** 可以google "get id_rsa" 生成公钥和私钥这个没搞明白 **


_以上方法我是没搞定_
**参考文献**[centos怎么配置ssh免密码](https://zhidao.baidu.com/question/1304780305953700019.html)

**我就有点晕，明明可以很简单的ssh-keygen 为什么要那么麻烦呢，秀操作啊老师MMP**

## 5-1~2
## 增加服务器安全性
> 如果你买了一个服务器没有修改的话登陆端口就是**22**虽然不知道你的用户名但是出于安全性考虑还是得修改这个

```bash
centos里面呢必须在防火墙中添加此端口不然还是访问不了
vi /etc/sysconfig/iptables
service iptables restart
sudo vi /etc/ssh/sshd_config
Port 31281
```

```bash
ssh -p 31281 root@192.168.0.119
```
[增加安全性参考文章](http://www.osyunwei.com/archives/672.html) 修改端口、禁止root用户登陆

> 什么都不用添加，更改用户权限就行了。

```bash
gpasswd -a imooc sudo
sudo visudo 
在root ALL=(ALL) ALL
下添加相同参数只需要修改用户名即可 好像也有个目录是sudoers
查到了在/etc/sudoers 目录下
```
**port 范围0~65536 但是0~1024最好不要使用，而且是必须用root身份启动. 1024~65536选择使用**
> 关闭root登陆权限，禁止密码登陆，禁止空密码

```bash
PermitRootLogin no  禁止root
PermitEmptyPasswords no 禁止空密码
passwordAuthentication no 禁止密码登陆
```
> ctrl+a 回到行首 ctrl+e 回到行尾

## 配置iptables  Fail2Ban

[yum (rpm) 和 apt-get的对应关系](http://blog.csdn.net/robertsong2004/article/details/36677689)
```bash
sudo apt-get update -y | sudo apt-get upgrade -y
yum makecache | yum update

sudo iptables -F
```

> iptables.up.rules 此文件里面内容如下

```
*filter
# 允许所有建立起来的链接
# allow all connections
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# 允许所有出去的流量
#allow out traffic
-A OUTPUT -j ACCEPT

# 允许https这样下的链接
#allow http https
-A INPUT -p tcp --dport 443 -j ACCEPT
-A INPUT -p tcp --dport 80 -j ACCEPT

# ssh登陆方式建立通道
#allow ssh port login
-A INPUT -p tcp -m state --state NEW --dport 31281 -j ACCEPT

# 方便测试服务器有没有停机，都是通过ping这种方式
#ping
-A INPUT -p icmp -m icmp --icmp-type 8 -j ACCEPT

# 日志
#log denied calls
-A INPUT -m limit --limit 5/min -j LOG --log-prefix "iptables denied:" --log-level 7

# 敏感规则对于恶意访问ip拦截
#drop incoming sensitive connections
-A INPUT -p tcp --dport 80 -i eth0 -m state --state NEW -m recent --set
#-A INPUT -p tcp --dport 80 -i eth0 -m state --state NEW -m recent --update --seconds 60 --hitcount 150 -j DROP  这段代码有问题 现在没搞懂这个配置文件所以也解决不了 失望

-A INPUT -p tcp --dport 80 -i eth0 -m state --state NEW -m recent --update --seconds 60 -j DROP

# 禁止所有其他进入服务器流量
#reject all other inbound
-A INPUT -j REJECT
-A FORWARD -j REJECT

COMMIT 
```

```bash
sudo iptables -F 不知道什么意思啊 反正搞出来了(经过深入学习这是一条清空防火墙规则的命令)
sudo iptables-restore < /etc/iptables.up.rules
sudo service iptables status 

关闭虚拟机防火墙：

关闭命令：  service iptables stop

永久关闭防火墙：chkconfig iptables off

两个命令同时运行，运行完成后查看防火墙关闭状态
service iptables status

1 关闭防火墙-----service iptables stop 
2 启动防火墙-----service iptables start 
3 重启防火墙-----service iptables restart 
4 查看防火墙状态--service iptables status 
5 永久关闭防火墙--chkconfig iptables off 
6 永久关闭后启用--chkconfig iptables on
```
> 创建sh脚本 每次重启后自己输入sudo iptables-restore < /etc/iptables.up.rules

```
sudo vi /etc/sysconfig/if-up.d/iptables
内容：
#!/bin/sh
iptables-restore /etc/iptables.up.rules
给执行权限
sudo chmod +x iptables
```

## 搞了几个小时02点17分 居然把iptables弄没见了？明天重新创建吧RLG

## 重新安装，重新来了一遍更熟练了
[Fail2ban源码安装方法](http://blog.csdn.net/Gekkoou/article/details/51119549)
> 源码安装

```bash
wget https://github.com/fail2ban/fail2ban/archive/0.9.3.tar.gz
tar -xzvf 0.9.3.tar.gz
cd fail2ban-0.9.3
python setup.py install
```

## 说实话 fail2ban这里了 00点55分 睡觉去了  明天下载ubuntu把 能把我累死，我也是醉了


## 经研究，还是用ubuntu系统把 重新记录新命令，以上均为centos命令

防火墙
```
和上面方法类似只不过命令有所改变
sudo ufw status 查看防火墙状态
sudo ufw enable 开启防火墙
sudo ufw disable 关闭防火墙

每次更改/etc/iptables.up.rules
都要重新
sudo iptables-restore < /etc/iptables.up.rules
```
Fail2ban 
```
sudo apt-get install fail2ban
sudo vi /etc/Fail2ban/jail.conf
 以下都更改
 # bantime = 600
 bantime = 3600
 
 改为自己邮箱
 destemail = 1004041672@qq.com

 #action = %(action_)s 搞不懂为什么要加mw
 action = %(action_mw)s
 
sudo service fail2ban status 查看状态 想要停掉 stop就好了
```
> 以上都是一些基础性的比较简单的安全配置，这些配置呢都算是基础之劳，但是可以让一台裸的服务器大大提高安全防护等级，虽然防护等级不够，但是抵御一些基础的东西还是ok的，想做更高级的东西需要做很多运维知识的储备，比如说可以给生产的服务器登陆设置ip的绑定或者是限制，只有特定内网的机器才能登陆这台服务器， 也就是通俗所讲的在内网假设一台跳板机 ，通过本地电脑连接跳板机再从跳板机登陆到服务器，

## 6-1~2
## 开始搭建环境了 呜呜呜呜呜 终于到了

```
首先再更新一边
sudo apt-get update
安装模块/包文件
安装了一大堆有些认识有些没见过
sudo apt-get install vim openssl build-essential libssl-dev wget curl git
到github 找到 nvm 下面有两种安装方式 wget curl 我们复制wget 建议安装好后再打开一个终端操作
nvm install v6.9.5  为什么安装的是node呢？
nvm use v6.9.5
nvm alias default 默认让系统里面的版本就是这个版本？难道是不让升级？

因为国内的原因，用npm太慢或者链接不上，改为淘宝的镜像下载
npm --registry=https://registry.npm.taobao.org install -g npm
原来npm -v可以查看这个镜像的更新程度啊 哈哈哈  老师视频的版本是4.2.0 目前2018-1-14版本号是5.6.0

增加系统文件监控次数
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p 

安装cnpm 有时候npm拉取不到的东西可以去cnpm下载，网速不行还是使用npm
npm --registry=https://registry.npm.taobao.org install -g cnpm
cnpm sync koa 同步一下 koa框架就会强制把国外的模块拿到国内去

安装全局工具包
npm i pm2 webpack gulp grunt-cli -g
vi app.js
	内容如下
	const http = require('http')

	http.createServer(function(req,res){
	        res.writeHead(200,{'Content-Type' : 'text/plain'})
	        res.end('来自慕课的力量')
	}).listen(8080)

	console.log('server runing on http://192.168.0.121:8080')	
80端口好像被占用了或者被禁用了 改成了8080
但是我是内网服务器，不能再公网访问到，很蛋疼

node app.js
sudo vi /etc/iptables.up.rules
#allow http https下增加一行格式一样更改端口号即可
sudo iptables-restore < /etc/iptables.up.rules
然后就可以通过ip+端口的方式访问到服务器上返回的纯文本

不过这样的话 ctrl+c 就会结束服务而且出现错误的话服务直接就停止了，所以我们需要用到pm2管理node

pm2 start app.js
pm2 list 查看本台服务器跑的服务
pm2 show app 更加详细的一个展示  这里是名字所以app.js不行
pm2 logs 查看实时日志
```
> 哇咔咔咔咔  原来用node搭建好就可以访问了啊 啊？？？？？？？？WDMY

## 下节预告 我们想要通过不带端口号的ip或者域名直接访问到80端口的服务，但是现在的用户权限又不足以让我们再80端口进行监听 下一节使用nginx来实现更加**魔法的效果**

## 7-1.mp4
## nginx
> 这里介绍下为什么要用到nginx，以及nginx的主要作用

```
原因是为什么的demoobug用户下node是不具备root的运行权限的，所以不能监听0~1024之间的任何一个端口，当然也包括80端口，如果强制通过sudo的话来启动node服务也不是不可以一个是需要额外的配置，一个是我们去放大node的一个权限，这多少会带来一些额外的风险和成本，我们先不通过域名来访问，先直接通过ip来达到访问的效果的话，怎么做呢：
我们就需要引入nginx用root级别的权限来启动对80端口的监听，同时吧80端口的流量分配给node服务的另一个端口实现这种服务的代理，如何服务器只需要一个网站程序的话，那解析网站到服务器的一个网址，网站程序监听80端口就可以了，如果是服务器有很多个应用，借助nginx的话不仅可以实现端口的代理，还可以实现负载均衡，让他来判断是来自哪个域名或者是来自IP的一个访问，可以根据配置的规则来准发给特定的端口，或者是特定的某几台机器，在我们这个案例中就是将80端口的请求转发到node启动的8080端口来处理
```
> 

```
sudo service apache2 stop  
sudo service apache stop   有些服务器会预装apache 暂停估计是有冲突？还是端口冲突？
update-rc.d -f apache2 remove  再来试试卸载
 Removing any system startup links for /etc/init.d/apache2 ...  这个的意思是有吗？

sudo apt-get remove apache2  然后再移除apache2  还真的有。。。

sudo apt-get update 然后更新包列表。。。

sudo apt-get install nginx   

nginx -v 稳定版本1.4.6 

cd /etc/nginx/conf.d 

sudo vi 696px-com-8080.conf   一目了然

	配置内容
	upstream 696px {
	 server 127.0.0.1:8080;
	}

	server {
	 listen 80;
	 server_name 192.168.43.7;

	 location / {
	  proxy_set_header X-Real-IP $remote_addr;
	  proxy_set_header X-Forward-For $proxy_add_x_forwarded_for;
	  proxy_set_header Host $http_host;
	  proxy_set_header X-Nginx-Proxy true;

	  proxy_pass http://696px;
	  proxy_redirect off;
	}
	}

sudo nginx -s reload

sudo vi /etc/nginx/nginx.conf
# server_tokens off; 取消注释

sudo service nginx reload 就好了


这样浏览器返回的头信息就service就是nginx了 没有了更详细的nginx 1.4.6(ubuntu)



```
> 好好回顾一下前面的东西，以后部署项目的话心里有点B数



## 8-1~2
## 主要就是域名解析以及七牛云的用法(七牛云耶终于找到可以用的地方了，第一次接触也不知道是啥时候？)

> 就是不知道DNSPOD比域名厂商免费提供的解析好多少。


## 9 1-2
## 安装mongoDB
>老师说可以购买一个现成的服务器，但是我这毕竟自己用，那样的话成本太大

> google搜索  install mongodb on ubuntu 找到后将manual改为V3.4(当然我没改了喽)



```
sudo vi /etc/apt/apt.conf 这个里面是阿里云镜像会导致安装失败，这里要注释掉(当然我的服务器是虚拟机的就不存在这样的问题)
 Acquire::http::Proxy "http://mirrors.aliyun.com";

sudo apt-get install -y mongodb-org
如果嫌下载速度慢的话可以终止掉去
sudo vi /etc/apt/sources.list.d/mongodb-org-3.6.list 把镜像地址修改为阿里云的
原：deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.6 multiverse
替：deb [ arch=amd64 ] https://mirrors.aliyun.com/mongodb/apt/ubuntu trusty/mongodb-org/3.6 multiverse

sudo service mongod start 开启服务
cat /var/log/mongodb/mongod.log 检查是否开启成功 

```

然而发现mongo命令启动不了，发现是防火墙问题
修改 /etc/iptables.up.rules
```
# mongodb connect
-A INPUT -s 127.0.0.1 -p tcp --destination-port 27017 -m state --state NEW,ESTABLISHED -j ACCEPT
-A OUTPUT -d 127.0.0.1 -p tcp --source-port 27017 -m state --state ESTABLISHED -j ACCEPT
```
sudo iptables-restore < /etc/iptables.up.rules 必须执行此条命令才可生效

> 全世界的人都知道mongodb跑在27017端口所以要修改
```
sudo vi /etc/mongod.conf  修改为8081
然后更新防火墙
原来修改后还要指定？那修改的意义是什么呢
mongo --port 8081
```

> 打包
```
tar zcvf linux.tar.gz linux
```
> 本地文件上传linux
```
scp -P 31281 ./linux.tar.gz demoorbug@192.168.1.106:/home/demoorbug/dbbackup
```

> 解压
```
tar xvf linux.tar.gz

```
> 备份数据库
```
mongodump -h 127.0.0.1:27017 -d 696px -0 696px.app.backup
```

> 导入mongodb数据库
```
mongorestore -p 8081 -d 696px ./dbbackup/696px.app.backup/696px
上面的命令报错了
mongorestore --host 127.0.0.1:8081 -d 696px ./dbbackup/696px.app.backup/696px
```

> 查看数据库是否导入成功
```
mongo
show dbs
use 696px
show tables
db.creations.find({})


db.users.find({})
```

> 导出单表数据
```
mongoexport -d imooc-movie -c users -q '{"name":{&ne:null}}' -o ./movie-users.json

```
> 导入单表数据
```
mongoimprot --host 127.0.0.1:8081 -d 696px -c users ./movie-users.json
```
> 删除已有数据库
```
mongo --host 127.0.0.1:8081 696px --eval "db.dropDatabase()"
```

## 管理mongodb安全性
> 最好是再数据库导入后再增加安全性

进入mongo
```
use admin
db.createUser({user: '696px_cases_owner',pwd: '1004041672',roles: [{role:'userAdminAnyDatabase', db:'admin'}]}) 越复杂越好

db.auth('696px_cases_owner','1004041672') 进行用户授权 1代表授权成功


use 696pppp
db.createUser({user: '696pppp-runner',pwd: '1004041672',roles: [{role:'readWrite', db:'696pppp'}]}) 拥有读写权限

db.createUser({user: '696pppp-wheel',pwd: '1004041672',roles: [{role:'read', db:'696pppp'}]}) 拥有读权限  

以上密码最好都不一样

然后要切换到admin 用admin用户授权
use admin
db.auth('696px_cases_owner','1004041672')

```
> 打开mongod配置文件修改security:
```
security:
  authorization: 'enabled'

重启
service mongod restart
mongo --port 8081
use admin
db.auth('696px_cases_owner','1004041672') 
```

> 直接进入数据库 带有操作权限的那种
```
mongo 127.0.0.1:8081/admin -u 696px_cases_owner -p 1004041672

这样就可以直接进去了。。
```
> 2018/1/21/01点39分 睡觉睡觉 溜了溜了

## 9-3~6
## 迁移数据库

> 如果服务器上线后有密码的如何导入导出呢？其实大家搜索一下官网的话就能自己摸索出来

```


```