---
title: Arch
date: 2022-03-22 10:11:52
tags: [Arch]
categories: [linux]
---

# 安装Arch linux 完整指南
## 制作Usb启动盘
下载最新的映像制作usb启动盘
MacOs:
首先识别usb设备, 打开终端输入:
```shell
diskutil list
```
找到usb设备,然后复制usb的标识符 类似于/dev/diskX, 然后卸载该设备,而不是弹出
```shell
diskutil unmountDisk /dev/diskX # 这里是你的设备号
```
然后执行该命令创建启动盘
```shell
dd if=archlinux-version-x86_64.iso of=/dev/rdiskX bs=1m # 这里设备号"rdiskX"就只是加了一个r,意思是可以传输速度更快
```
运行完后会显示无法读取设备, 点击忽略即可

插入u盘,选择u盘启动就可以进行下一步
## 设置网络
首先确保拥有网络,不然没法进展
```shell
ip addr show # 如果有有线网络就会显示ip, 无线网络需要手动连接, 连接不了可能是驱动不支持
# 连接wifi
iwctl
device list # 显示无线设备
station wlan0 scan # 启动wlan0设备进行wifi扫描
station wlan0 get-networks # 显示可用的wifi
station wlan0 connect "wifi-2.5g" # 连接该wifi
ip addr show # 查看是否连接成功
ping www.baidu.com # 查看是否可以ping
```
## 设置硬盘, 仅适用于UEFI启动模式
请备份好硬盘数据,以免重要数据丢失
```shell
fdisk -l # 查看硬盘设备
fdisk /dev/sda # 这里选择自己要安装系统的硬盘设备号
p # 查看所有分区
d # 删除分区 一直回车即可
g # 创建gpt分区表
n # 创建第一个分区, 两个对话都直接enter默认值, 第三个选择大小输入:+500M
t # 设置分区格式为EFI system, 因为只有一个分区, 所以第一个选项默认为1, 第二个选项输入1 代表EFI, 输入L可以查看所有的
p # 查看是否创建成功
# 接着创建第二个分区
n # 这次全部选项都按enter, 表示这个分区会使用所有空间
t # 2, 30 设置为linux Lvm 
p # 这时候你会看到第一个分区格式为EFI, 第二个分区为linux Lvm 代表成功设置硬盘
```
格式化硬盘
```shell
mkfs.fat -F32 /dev/sda1
pvcreate --dataalignment 1m /dev/sda2
vgcreate volgroup0 /dev/sda2
lvcreate -L 30GB volgroup0 -n lv_root
lvcreate -l 100%FREE volgroup0 -n lv_home
# 激活
modprobe dm_mod # 这会将内核模块加载到内核中
vgscan 
# Reading volume groups from cache.
# Found volume group "volgroup0"using metadata type lvm2
vgchange -ay 
# 2 logical volume(s)in volume group "volgroupo"now active
# 继续格式化
mkfs.ext4 /dev/volgroup0/lv_root
mount /dev/volgroup0/lv_root /mnt # 挂载
mkfs.ext4 /dev/volgroup0/lv_home 
mkdir /mnt/home
mount /dev/volgroup0/lv_home /mnt/home # 挂载
mkdir /mnt/etc
genfstab -U -p /mnt >> /mnt/etc/fstab
cat /mnt/etc/fstab # 此命了输出两个目录就表示设置正确lv_root and lv_home
```

## 增加下载速度
因为源的顺序是随机的,没办法保证速度
```shell
sudo pacman -S reflector
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak # 备份,以免出问题
sudo reflector --verbose --latest 5 --sort rate --save /etc/pacman.d/mirrorlist
sudo pacman -Syu # 速度直接递增,以前几十kb, 现在能到几m
```

另一个方法也设置过,但是没成功,也记录下来吧
```shell
pacman -Syy
pacman -S reflector
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak # 备份,以免出问题
reflector -c "US" -f 12 -l 10 -n 12 --save /etc/pacman.d/mirrorlist
```

## 安装Arch
```shell
pacstrap -i /mnt base # 大概478.31MiB 一直回车即可
arch-chroot /mnt # 现在就切换到了自己的系统了
pacman -S linux linux-headers linux-lts linux-lts-header #lts长期支持稳定版 不带后缀表示最新版 ,这里都安装, 搞坏了还可以切换内核. 安装了所有你就可以在启动时在它们直接选择, 如果存在某种不兼容当你更新内核时,你的系统将无法启动, 你就可以简单的选择另一个内核
pacman -S vim
pacman -S base-devel openssh # 这俩包都是可选的base-devel 是一个包组,  它将安装一堆与开发相关的软件包,有时候需要这些软件包,所以不妨安装它, openssh将使我能够远程管理我的安装
systemctl enable sshd # 这将确保openssh在你的计算机启动时启动
```
开机启动wifi的包
```shell
pacman -S networkmanager wpa_supplicant wireless_tools netctl # 不知道为什么为这里没有100m大小, 只有几十m, 可能这就是导致我启动没有自动连接wifi的原因
pacman -S dialog # 可选包, 该包只是对话框, 对话框要做的是给我们的图像用户界面无法正常工作的情况下, 我们能够使用类似wifi菜单,并通过命令行连接到wifi. 安装的时候有一个链接没有响应,但没关系它确实找到了软件并安装了软件
systemctl enable NetworkManager # 启用网络管理器
```
## 设置lvm
```shell
pacman -S lvm2
# 为了确保我们的引导过程支持我们的配置,我们必须更改实际配置文件
vim /etc/mkinitcpio.conf
# 找到HOOKS=开头的行, 前面没有注释
# 改为: HOOKS=(base udev autodetect modconf block lvm2 filesystems keyboard ...) 只用添加lvm2即可,其他源代码...省略了
mkinitcpio -p linux-lts # 让配置生效
mkinitcpio -p linux

vim /etc/locale.gen # 找到自己的语言环境, 这里可以选择ZH或en_US.UTF-8 UTF-8把前面的注释去掉即可
locale-gen # 生成未注释的语言环境
# 设置密码
passwd #这是设置root用户
# 创建用户
useradd -m -g users -G wheel debug # debug替换成自己的用户名
passwd debug # 设置密码
# 确保安装了sudo
pacman -S sudo # 这里显示 -- reinstallling 代表系统已经有了
EDITOR=vim visudo # 把%wheel ALL=(ALL) ALL 前面的注释去掉
```
## 安装grub引导程序, 仅适用于EFI
```shell
pacman -S grub efibootmgr dosfstools os-prober mtools
mkdir /boot/EFI
mount /dev/sda1 /boot/EFI

grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck
# Installing for x86 64-efi platform
# Installation finished.No error reported
cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo # 启动屏幕使用英文, 就会出现几秒,所以没必要设置为别的
grub-mkconfig -o /boot/grub/grub.cfg # 现在就完成了一切
```
卸载硬盘and重启
```shell
exit
umount -a
reboot
```
## 创建交换 (Post install Tweaks)
```shell
su #切换到root
dd if=/dev/zero of=/swapfile bs=1M count=2048 status=progress
chmod 600 /swapfile
mkswap /swapfile
cp /etc/fstab /etc/fstab.bak # 以防万一 备份
echo '/swapfile none swap sw 0 0' | tee -a /etc/fstab
cat /etc/fstab # 将显示4个驱动器 lv_root, sda2 , lv_home, swapfile
free -m # 他被设置为0
mount -a # 测试确保没有错误
swapon -a 
free -m # 以前是0, 现在是2047
```
## 设置时区
```shell
timedatectl list-timezones # 找到自己的地区, 中国就设置为 Asia/Shanghai即可
timedatectl set-timezone Asia/Shanghai
systemctl enable systemd-timesyncd #确保下次启动时同步时钟
```
## 设置主机名
```shell
hostnamectl set-hostname myarchdebug 
cat /etc/hostname
vim /etc/hosts
127.0.0.1 localhost
127.0.0.1 myarchdebug
hostnamectl # 可以看到主机名
```

## 安装驱动
```shell
pacman -S intel-ucode # 这是可选的
# 使用xorg 单Wayland即将到来
pacman -S xorg-server 
# 我们仍然需要一个显卡驱动器
pacman -S nvidia-lts nvidia # 这里安装两个是因为内核安装了两个, 有可能我的显卡没法用, 可能要安装特定的驱动

```


(Arch Linux: Full Installation Guide - A complete tutorial/walkthrough in one video!)[https://www.youtube.com/watch?v=DPLnBPM4DhI&list=LL&index=9]






















# Arch install DWM 
(DWM initial setup and config. Icons, Bar Scripts, Keybindings, and Theming.)[https://www.youtube.com/watch?v=zaRzOEoyR4s&t=123s]
安装一些依赖, 不过和我所看视频有所不一样,这是针对他所在的Ubuntu环境

```
sudo apt install git vim xorg xserver-xorg linux-headers-4.19.0-9-amd64 firmware-linux-free mesa-utils wget curl build-essential i3lock imagemagick feh fonts-font-awesome


```
DWM
```shell
wget https://dl.suckless.org/dwm/dwm-6.3.tar.gz
tar -xzf dwm-6.3.tar.gz
mkdir suckless
mv dwm-6.3 suckless/
# 创建目录
mkdir Pictures Downloads Documents Videos bin .config
# 安装所需依赖
sudo apt install libxft-dev libxinerama-dev libxll-dev
cd ~/suckless/dwm-6.3
make

ln -sf ~/suckless/dwm-6.3/dwm ~/bin/dwm
cd ..

cd st # 这玩意我没有啊
make

ln -sf ~/suckless/st/st ~/bin/st
ln -sf ~/suckless/st/st-copyout ~/bin/st-copyout
sudo reboot

vim .xinitrc
exec dwm # 文件写入此代码, 保存并退出
start # 这样就启动了dwm
alt+shift+enter # 启动终端
```

以上的教程有缺失,没有缺失的视频约2小时,故放弃

# Arch DWM for SavvyNik
(Arch DWM)[https://www.youtube.com/watch?v=jD8BtmMK0do]

```shell
sudo pacman -Syu base-devel git vim # 安装了base, 不知道还需不需要base-devel
cd ~
git clone https://git.suckless.org/dwm
git clone https://git.suckless.org/st
# install依赖帮助我们编译dwm and st
sudo pacman -Syu xorg-server xorg-xinit libx11 libxinerama libxft webkit2gtk

cd ~
vim .xinitrc
exec dwm # 添加
# 开始编译
cd st
sudo make clean install
cd ~/dwm
sudo make clean install
# sudo vim config.h # 不用修改也可以,所以就不改了
# 找到55行的 "/bin/sh" 这里的意思是使用sh终端,但是我们需要使用刚刚编译好的新终端
# 更改为 "/usr/local/bin/st" 
# 获取目录的方式为 which st
cd ~
vim .bash_profile
#
# ~/.bash_profile
#
#
# [[ -f ~/.bashrc ]] && . ~/.bashrc
# startx 添加这么一行代码
exit # 重新登陆用户
# 现在已经是dwm界面了
cd dwm 
sudo vim config.h
# 修改第八行 *fonts[] = { "monospace:size=10" }  这里数字改为16
# 修改第九行 dmenufont[] = { "monospace:size=10" }  这里数字改为16
sudo make clean install # 每修改一次改一次代码,编译一次,是不是有些麻烦
exit # 重新登陆
alt+shift+q # 完全退出
```

# 解决出错问题
## Failed to load module "intel"
这个问题安装:
```shell
sudo pacman -Q xf86-video-intel
# 如果没有到话, 就安装
sudo pacman -S xf86-video-intel
```
## Failed to load module "nouveau"
```shell
sudo pacman -S xf86-video-nouveau
```
## 还有修改config.h 55行, 又改回了以前的/bash/sh 应该和这个没有关系

## 安装 or 卸载 gnome
安装
```shell
sudo pacman -S gnome
sudo pacman -S gnome-tweaks
systemctl enable gdm
# Created symlink /etc/systemd/system/display-manager.service -/usr/lib/systemd/system/gdm.service.
reboot
```
卸载
```shell
pacman -Rsc gnome # 卸载gnome及其依赖, Note: 此操作是递归的,可能会删除许多可能需要的包
# 此操作需格外小心, 执行后需重新安装某些包, 不然程序会出错
rm -rf /etc/systemd/system/display-manager.service
```
## 安装显卡驱动
```bash
sudo pacman -S nvidia nvidia-lts # 大部分显卡都可以这样,一些例外自动查询包可能不起作用, 就需要自己去找相应的源
```
安装470xx驱动
```bash
sudo git https://aur.archlinux.org/nvidia-470xx-utils.git
cd nvidia-470xx-utils
makepkg -si
# 遇到大坑, 不知道为什么移动网络访问不了nvidia驱动下载地址
# 解决办法就是用联通手机开启usb共享网络,临时解决
```
或者下载官方.run驱动程序,安装
```bash
wget https://us.download.nvidia.com/XFree86/Linux-x86_64/470.103.01/NVIDIA-Linux-x86_64-470.103.01.run
sudo chmod +X NVIDIA-Linux-x86_64-470.103.01.run
sudo sh NVIDIA-Linux-x86_64-470.103.01.run
```

# arch wifi
## 使用networkmanager
依赖项前面有讲
```bash
sudo systemctl enable NetworkManager # 启动networkmanager管理网络，很方便呢
sudo systemctl enable wpa_supplicant
sudo reboot

sudo nmtui # 图形界面管理
sudo nmcli # 命令行管理
```
## 使用iwd
[NetworkManager/iwd - Debian Wiki](https://wiki.debian.org/NetworkManager/iwd)
如果要用这个则必须关闭NetworkManager
```bash
sudo systemctl stop NetworkManager 
sudo systemctl disable --now wpa_supplicant
sudo systemctl restart NetworkManager

sudo pacman -S iwd
sudo systemctl enable iwd
sudo reboot
iwctl 
dvice list 
# 后续用法上面也有讲
```

## 驱动安装
这是最关键的，没驱动就会找不到网络
[网卡对应驱动查询 https://wireless.wiki.kernel.org/en/users/Drivers/b43#Supported_devices](https://wireless.wiki.kernel.org/en/users/Drivers/b43#Supported_devices)
```bash
lspci -k # 找到Network这一项，看看有没有kernel字段，如果有的话则表示有驱动，没有就必须根据网卡型号安装对应驱动
```
```txt
Network controller: Broadcom Inc. and subsidiaries BCM43142 802.11b/g/n(rev 01)
```
这是我的驱动型号，根据查询得知需要使用broadcom-wl驱动，但是呢，使用
`sudo pacman -S broadcom-wl`安装并重启，再输入`lspci -k`并没有加载，有可能是我前面安装的`realtek-firmware`这是通过`aur`方式安装的。
解决办法：
先卸载以前装的这些
```bash
sudo pacman -Rcns broadcom-wl
sudo pacman -Rcns realtek-firmware
```
[Arch Linux 上 的 broadcom-wl-dkms](https://linux-packages.com/arch-linux/package/broadcom-wl-dkms)
通过安装broadcom-wl-dkms解决了我的问题
```bash
sudo pacman -S broadcom-wl-dkms
```

## 其中安装realtek-firmware遇到了`ERROR: One or more PGP signatures could not be verified, arch linux`报错，解决方法如下

这一步不知道是不是必须
`gpg --generate-key` or `gpg --full-gen-key`
我用的第一个命令，创建一个key，然后继续
例如这里报错 FAILED (unknown public key A2C794A986419D8A)
我们就可以用这条命令
`gpg --receive-keys A2C794A986419D8A`
当然了，这里肯定又报错了
`gpg: keyserver receive failed: Server indicated a failure`
解决方法：
编辑此文件 ~/.gnupg/gpg.conf 
写入如下内容
```
no-greeting
no-permission-warning
lock-never
keyserver-options timeout=10
keyserver hkp://keyserver.ubuntu.com:80
```
[https://www.youtube.com/watch?v=_ANJMBryemY参看视频(https://www.youtube.com/watch?v=_ANJMBryemY])
视频中用的地址都不行，这个地址取自https://unix.stackexchange.com/a/399091
`gpg --generate-key`参考https://superuser.com/a/1210759

# 问题
有些bug需要安装久版本的包，但是呢pacman不提供， 这个网址到时有提供https://archive.archlinux.org/packages/ 安装方法
```bash
wget https://archive.archlinux.org/packages/l/l3afpad/l3afpad-0.8.18.1.11-5-x86_64.pkg.tar.zst
pacman -U l3afpad-0.8.18.1.11-5-x86_64.pkg.tar.zst
```

Error while loading shared libraries: libicuuc.so.59:
这个问题，通过`pacman -Syy`可以解决 
`pacman -Syu` 更新源，并升级


https://forum.manjaro.org/t/gedit-plugin-warning-when-opening-from-terminal/75437
```
** (gedit:3892): WARNING **: 12:37:59.339: Error loading plugin: libaspell.so.15: cannot open shared object file: No such file or directory

** (gedit:3892): WARNING **: 12:37:59.339: Error loading plugin: libhspell.so.0: cannot open shared object file: No such file or directory

** (gedit:3892): WARNING **: 12:37:59.340: Error loading plugin: libnuspell.so.5: cannot open shared object file: No such file or directory

** (gedit:3892): WARNING **: 12:37:59.340: Error loading plugin: libvoikko.so.1: cannot open shared object file: No such file or directory

** (gedit:3892): WARNING **: 12:37:59.341: Error loading plugin: libaspell.so.15: cannot open shared object file: No such file or directory

** (gedit:3892): WARNING **: 12:37:59.341: Error loading plugin: libhspell.so.0: cannot open shared object file: No such file or directory

** (gedit:3892): WARNING **: 12:37:59.341: Error loading plugin: libnuspell.so.5: cannot open shared object file: No such file or directory

** (gedit:3892): WARNING **: 12:37:59.341: Error loading plugin: libvoikko.so.1: cannot open shared object file: No such file or directory

```
解决方案：
```
sudo pacman -Syyu libvoikko hspell nuspell hunspell aspell
```

`GLIBCXX_3.4.30` ,这个问题看一个回答说的是服务端错误，把依赖这个项目降级，参考上面的方法

所需密钥丢失： https://github.com/FZUG/repo/issues/73#issuecomment-195919397
error: key "F9F9FA97A403F63E" could not be looked up remotely
error: required key missing from keyring
解决方法1： 删除密钥检查

vim /etc/pacman.conf
#SigLevel = Required DatabaseOptional
SigLevel = Never


解决方法2： 引入key， 当然了，第二种方法报网络错误了，所以用了第一种
sudo pacman-key --init
sudo pacman-key -r 7F2D434B9741E8AC

# 安装输入法
pacman -S fcitx fcitx-im fcitx-configtool

/etc/profile文件，在文件开头加入三行：

export XMODIFIERS="@im=fcitx"
export GTK_IM_MODULE="fcitx"
export QT_IM_MODULE="fcitx"

~/.pam_environment 添加：
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=\@im=fcitx

因为用的xorg，所以在~/.xinitrc文件添加fcitx，登录即可启动输入法，当然要在exec dwm之前加此代码

## 以前安装的是fcitx，换成fcitx5
https://wiki.archlinux.org/title/Fcitx5_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)
```bash
sudo pacman -Rcns fcitx
sudo pacman -S fcitx5-im fcitx-chinese-addons # 其中呢fcitx-im里面就包含fcitx5，fcitx5-configtool，qt，gtk。 fcitx-chinese-addons 包含所用到的语言框架，比如五笔，拼音之类
yay -S fcitx5-skin-fluentdark-git # 这是皮肤，用着还不错
```
我用的是pinyin这个中文框架
开启了云联想，源改为baidu，并且安装了离线字库
```bash
sudo pacman -S Fcitx5-pinyin-moegirl fcitx5-pinyin-zhwiki # 这俩是离线字库
```
## unicode
https://wiki.archlinux.org/title/Fonts#Non-latin_scripts
ttf-joypixels unicode图标，最后还是没用，用微软的解决方案
libxft-bgra 这个包可以解决dwm用unicode崩溃问题



## 用到的一些命令
lspci -k 查看驱动是否安装，有ker类似字样表示安装了驱动
fc-list 查看安装的字体及字体名称，格式
## 配置字体
这里没想到居然还有说法的
```bash
sudo pacman -S wqy-microhei nerdfonts 
sudo pacman -Ss nerd # 搜索相关字体，我用的是Hurmit，用pacmans -S 字体名下载即可
fc-list | grep "WenQuanYi"
fc-list | grep "Hurmit"
```
把相关信息填入dwm和st的字体配置文件中即可，比如dwm里面可以填写多条，Hurmit Nerd Font:pixelsize=14:antialias=true:auohint=true:type= 这里的type信息根据fc-list对应字体的style填写
WenQuanYi Micro Hei:size:10:type=Regular:antialias=true:authint=true
差不多如此即可  

## 如果只安装wqy字体就会导致很多字没有，比如“䵈”字就没有，目前只能通过复制微软字体库解决
```bash
# 把win字体打包成zip，通过scp发送到linux
scp Fonts.zip usename@ip:~
# linux中解压
unzip Fonts.zip
sudo cp -r Fonts/. /usr/share/fonts/win-font
fc-list | less # 查看是否安装
```

# 安装yay，以前只知道手动aur安装，yay就是替代手动安装，只要输入yay有的包，aur就会自己安装对应程序
https://wiki.archlinux.org/title/General_recommendations_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87) 此链接最后有推荐的大陆软件集合，挺不错的，比如微信，网易云

安装方法：
```bash
pacman -S git base-devel 
git clone https://aur.archlinux.org/yay-bin.git
cd yay-bin
makepkg -si
```
自己安装了，google-chrome和netease-cloud-music，都挺不错的

# 安装了qv2ray 
通过导入链接形式添加节点关闭后节点消失，问题是我用的旧vmess所以才这样，设置里面勾选旧vmess或者手动新建节点即可
配置开机启动呢，也很简单，在.xinitrc里面添加`qv2ray &` 再在~/dwm/config.h中添加
```c
static const Rule rules[] = {
    { "qv2ray", NULL, NULL , 1 << 8, 1, -1}
}
```
这个qv2ray如何来的呢，官网有介绍https://dwm.suckless.org/customisation/rules/
```bash
xprop | awk '
	/^WM_CLASS/{sub(/.* =/, "instance:"); sub(/,/, "\nclass:"); print}
	/^WM_NAME/{sub(/.* =/, "title:"); print}'
```
终端输入此命令，然后鼠标点击对应的应用窗口即可得到class名字

莫名其妙的yay安装突然找不到qv2ray，输入yay -S qv2ray直接安装的是qv2ray-dev-git，必须把此命令改成yay -Sa qv2ray， -a的意思是从aur中寻找，不带的话就是从--repo中寻找，莫名其妙的

安装uvw v2.11 这个居然可以这么简单，我是没想到的
首先呢，通过yay下载aur上的pkg，也可以通过git下载
```bash
yay -G uvw
git checkout ed67184 #如果安装2.10就切换到对应分支即可
makepkg -si 
```
然和配置/etc/pacman.conf 忽略uvw的更先即可
`sudo /etc/pacman.conf`
修改如下： IgnorePkg =  uvw

# 分辨率
```bash
xrandr -q #查看分辨率
#我的第一个显示器叫 eDP1，把这里Virtual-1
xrandr --output Virtual-1 --mode 1920*1080 --rate 60 # 设置分辨率
```
# 壁纸
```bash
feh --bg-fill --randomize /usr/share/backgrounds/archlinux/* #这个文件夹每次随机壁纸
# 注意，启动项里面分辨率要在壁纸之前设置
```

# 音频配置

我默认已经有音频驱动
lspci -k | less
```bash
sudo pacman -S alsa-utils # 音频管理
amixer sget Master # 并没有显示音频

aplay -l 里面有很多设备，找到HDA Intel PCH自己对应的设备号
```
编辑/etc/asound.conf或者~/.asoundrc
```bash
defaults.pcm.card 1 # 对应aplay中
defaults.pcm.device 0 # 对应aplay中
defaults.ctl.card 1 
```
pcm选项决定用来播放音频的设备，而ctl选项决定那个声卡能够由控制工具（如 alsamixer）使用。
重启即可，或者重启音频设备
https://segmentfault.com/a/1190000002918394
# slstatus 配置
打开config.h
```bash
static const struct arg args[] = {
        /* function format          argument */
        { disk_free, "%s 丨 ",           "/home" },
        { cpu_perc, "%s%% 丨 ",           NULL },
        { ram_perc, "%s%% 丨 ",           NULL },
        { run_command, "%s 丨 ",         "amixer sget Master | awk -F '[][]' '{ print $2 }' | sed '/^\s*$/d'" },
        { run_command, "%s 丨 ",         " light -G | awk '{if( $1 == 100)  {print 100} else {print 1+int($1)}}'" },
        { datetime, "%s ",           "%m/%d %R" },
};
```
第一个磁盘空间
第二个cpu占用
第三内存占用
第四个是通过amixer显示的音量
第五个通过light显示屏幕亮度
第六个显示当前时间年月时间
[c date time](https://zetcode.com/articles/cdatetime/)
[awk if语法](https://blog.csdn.net/xiyangyang052/article/details/45462505)
[awk 运算符](https://blog.csdn.net/xiyangyang052/article/details/45675027)

# 屏幕亮度配置用的是light，当然这是笔记本控制亮度，如果有/sys/class/backlight/intel_backlight/ 目录就可以使用light
https://wiki.archlinux.org/title/Backlight_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)
https://wiki.archlinux.org/title/Backlight_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#Backlight_utilities
```bash
sudo pacman -S light
sudo usermod -a -G video [YourUserName] # 这里把用户加入video用户组，否则运行light调节亮度必须管理员权限
light -A 10 # 增加10亮度
light -U 10 # 减少10亮度
```

# 配置dwm快捷键设置音量及屏幕亮度

dwm目录下添加lightup.sh,lightdown.sh
```bash
# lightup.sh
#!/bin/bash
light -A 10

# lightdown.sh
#!/bin/bash
light -U 10
```
```bash
chmod +x lightup.sh lightdown.sh #授予执行权限
```
编辑config.h
```c
static const char *lightup[] = { "/home/debug/.config/dwm/lightup.sh", NULL }  
static Key keys[] = {
	{MODKEY, XK_F3, spawn, {.v = lightup}}
}
```
其他同理

# 配置开机投屏的设置
```bash
xrandr # 打印显示器信息

Screen 0: minimum 8 x 8, current 1920 x 1080, maximum 32767 x 32767
eDP1 connected primary (normal left inverted right x axis y axis)
   1366x768      59.99 +  39.94
   1280x720      59.86    60.00    59.74
   1024x768      60.00
   1024x576      60.00    59.90    59.82
   960x540       60.00    59.63    59.82
   800x600       60.32    56.25
   864x486       60.00    59.92    59.57
   640x480       59.94
   720x405       59.51    60.00    58.99
   680x384       60.00
   640x360       59.84    59.32    60.00
HDMI1 connected 1920x1080+0+0 (normal left inverted right x axis y axis) 480mm x 270mm
   1920x1080     60.00*+  59.94
   1280x1024     60.02
   1440x900      59.90
   1280x800      59.91
   1152x864      75.00
   1280x720      60.00    59.94
   1024x768      70.07    60.00
   800x600       60.32    56.25
   720x480       60.00    59.94
   640x480       66.67    60.00    59.94
   720x400       70.08
VIRTUAL1 disconnected (normal left inverted right x axis y axis)

```
其中eDP1是我的笔记本显示器， HDMI1是外接显示器
```bash
xrandr --output HDMI1 --primary --auto --output eDP1 --off # 此命令把外接显示器设置为主显示器（因为笔记本是720p的，不设置主显示器就会导致屏幕留白，或者设置--left-of eDP1 也可以解决留白问题），顺带把笔记本显示器关闭
```
然后就是每次开机后自动设置投屏及分辨率
在/etc/X11/xorg.conf.d/ 下创建10-monitor.conf文件，编辑如下内容
```base
Section "Monitor"
	Identifier "HDMI1"
	## 设置分辨率为1920x1080
	Option	"PreferredMode" "1920x1080"
	## 设置为左副屏，因为Primary不知道为什么不起作用，只能用这个代替，不过效果都一样，可以接受
	Option	"LeftOf" "eDP1"
EndSection

Section "Monitor"
	Identifier  "eDP1"
	## 禁用笔记本屏幕
	Option	"Disable" "true"
EndSection
```
PS：
[此文章有热插拔投屏设置详情](https://blog.csdn.net/liberty1997/article/details/88658763)
[如上配置文件参考](https://gist.github.com/r0xsh/4d1c7219ee63e4cee0c7fe1077559b28)
[如上配置文件参考2](https://www.youtube.com/watch?v=lhiLWxJgiAo)


# DWM官方文档
要启动dwm, 理想情况下你应该设置一个~/.xinitrc, 其中至少有exec dwm

