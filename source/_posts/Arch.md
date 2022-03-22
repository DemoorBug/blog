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
sudo vim config.h
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


