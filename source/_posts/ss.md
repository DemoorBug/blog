---
title: ss
date: 2018-12-22 22:08:33
tags: [ss,ubuntu,速锐]
categories: [ss]
---


# bbr+kcptun+ss 搭建（ubuntu14）
kcptun 待验证
## 1. bbr 开启可以有效提高网速

`sudo apt-get update`  //更新源
`sudo apt-get upgrade`  //升级系统

## 2. 开启bbr
- 下载内核
  - `wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.14.12/linux-image-4.14.12-041412-generic_4.14.12-041412.201801051649_amd64.deb`
- 内核安装
  -`dpkg -i linux-image-4.*.deb`
- 查看内核版本
`dpkg -l | grep linux-image`
- 删除旧内核,除4.`*`版本外全部删除即可
  - `apt-get purge 旧内核`
- 更新grub系统引导文件并重启
  - `update-grub`
- 重启服务器
  - `reboot`

## 3. 开机后开启bbr
- 查看内核 ≥ 4.9
  - uname -r
- 执行
  - `echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf`
  - `echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf`
- 保存生效
  - `sysctl -p`
- 执行
  - `sysctl net.ipv4.tcp_available_congestion_control`
  - `sysctl net.ipv4.tcp_congestion_control`
  - 如果结果都有bbr，证明内核已开启bbr
- 执行
  - `lsmod | grep bbr`
  - 看到 tcp_bbr 模块即说明 bbr 已启动

还可以继续安装kcptun，不过上面的足以日常使用，不喜欢折腾的就不用搞

## 4. 搭建ss
- 安装
  - `apt install python3-pip`
  - `pip3 install setuptools`
  - `pip3 install distribute`
  - `pip3 install shadowsocks`
- 添加ss配置文件
  - vi /etc/shadowsocks.json
```
{
"server":"替换为你服务器ip地址",  
"server_port":端口,
"local_port":1080,
"password":"密码",
"timeout":600,
"method":"aes-256-cfb"
}
```
将里面的内容为中文的部分替换为相应的值

- 开启ss服务
  - `ssserver -c /etc/shadowsocks.json -d start`

**到这里就可以结束了，后续的我自己没搞定，参考**

## 5. kcptun配置
- 安装kcptun
  - `mkdir kcptun`
  - `cd kcptun`
  - `wget https://github.com/xtaci/kcptun/releases/download/v20170904/kcptun-linux-amd64-20170904.tar.gz`
  - `tar zxvf kcptun-linux-amd64-20170904.tar.gz`
  - `rm -f kcptun-linux-amd64-20170904.tar.gz`

## 6. 开启 kcptun 服务

- `./server_linux_amd64 -t "服务器ip:shadowsocks设置的端口" -l ":4001" --key 密码 --log 4001.log &`
  - & 表示后台运行

## 7. 本地配置
- 下载相对应的客户端文件。（如上面server安装的是v20170904的kcptun，client就下载对应版本的）
  - [下载地址](https://github.com/xtaci/kcptun/releases?after=v20171129)
- 新建文件client.json
```
{
"localaddr": ":1082", //避免与本机占用的端口冲突即可

"remoteaddr": "your server ip : kcptun 端口号", //对于上述配置是 x.x.x.x:4001

"key": "kcptun密码", //server上kcptun设置的密码

"crypt": "aes",

"mode": "fast2",

"conn": 1,

"mtu": 1350,

"sndwnd": 512,

"rcvwnd": 512,

"nocomp": false
}
```
- 新建start.bat内容如下
  - `client_windows_amd64.exe -c client.json`
  - 将kcptun的文件client_windows_amd64.exe和新建的文件放到同一目录之下









## 之前使用的方法

这种是一键安装
```bash
wget xiaofd.github.io/ruisu.sh && bash ruisu.sh
```

更换内核：
1. 查看内核
  - `uname -r`
2. 安装新内核
  - `sudo apt-get install linux-image-extra-3.13.0-24-generic`
3. 卸载其他内核
  - 查看系统现有内核
    - `dpkg -l|grep linux-image`
  - 卸载列出的其他内核
    - `sudo apt-get purge linux-image-3.16.0-36-generic linux-image-extra-3.16.0-36-generic`
4.  更新grub系统引导文件并重启
  - `sudo update-grub `
  - `sudo reboot`

速锐操作命令
```
service serverSpeeder start #启动
service serverSpeeder stop #停止
service serverSpeeder reload #重新加载配置
service serverSpeeder restart #重启
service serverSpeeder status #状态
service serverSpeeder stats #统计
service serverSpeeder renewLic #更新许可文件
service serverSpeeder update #更新
chattr -i /serverspeeder/etc/apx* && /serverspeeder/bin/serverSpeeder.sh uninstall -f #卸载
```

shadowsocks启动
```
sudo ssserver -c /etc/shadowsocks.json -d start

后台运行：ssserver -c /etc/shadowsocks.json -d start

后台停止：ssserver -c /etc/shadowsocks.json -d stop

前台运行：ssserver -c /etc/shadowsocks/config.json

```


<!-- {
"server":"",
"server_port":31241,
"local_port":1080,
"password":"@a1004041672A.@",
"timeout":600,
"method":"aes-256-cfb"
}


./server_linux_amd64 -t ":31241" -l ":4001" --key "@a1004041672A.@" --log 4001.log -->

# 问题
ubuntu 16出现的问题
## Command "python setup.py egg_info" failed with error code 问题
```bash
pip install setuptools
pip install distribute
```
我用这条命令之前还用了很多命令
```bash
apt-get remove python*

apt-get install python python-pip
// 如果pip用不了，则可以这样用
python -m pip install 软件名
// 或
python -m pip install --upgrade --force pip // 这条命令好像式升级来着
```
## pip升级后Import Error:cannot import name main解决方案
方法1
```bash
vi /usr/bin/pip3
```
将原来的
`from pip import main` 改为 `from pip._internal import main`
