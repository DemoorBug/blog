---
title: Mac hyper key 仅需要使用hammerspoon就可实现, 可配合Alfred, 不需要借助Karabiner-Elements 和 BetterTouchTool | Mac hyper key only requires hammerspoon, works with Alfred and does not require Karabiner-Elements or BetterTouchTool.
date: 2022-02-18 15:04:37
tags: [Mac]
categories: [Mac, hammerspoon, Alfred]
---

# 为什么不使用Karabiner-Elements 和 BetterTouchTool
**Karabiner-Elements**
我一直在使用[Karabiner-Elements](https://karabiner-elements.pqrs.org/)软件绑定我的键位以及设置Hyper key, 但是有时候这个软件会出bug,比如说偶尔开机会导致软件的映射全部失效,必须重新安装或者重启电脑,这真的很烦人,并且软件更新频率很低, 我这个bug已经好几个月没有解决了,索性就换一款类似产品用,不然太糟心了
**BetterTouchTool**
还是bug问题,我是将`Caps_lock`映射为`Control`,`Control`映射为`Caps_lock`,`Hyper key`我是用的`Caps_lock`映射的(这里可能有点绕),在Karabiner-Elements中功能是正常的,大家各司其职,但是呢这款软件会把以上两个键位全部映射为`Hyper key`,所以最后导致放弃,软件大概有100m左右,我就是要一个小功能而已,完全没必要,折腾了一小时无果卸载了

# 用hidutil 和 hammerspoon 实现Hyper key
**hidutil**
首先打开这个网址[hidutil key remapping generator for MacOS](https://hidutil-generator.netlify.app/)

选择From key,To key设置自己需要映射的快捷键,我这里遇到一个坑,就是left_command实际上映射的是右边的command的键,这里大家最好自己实践一下
left_command escape 这里实际上映射的是右command
application fn 因为用的是win键盘,mac上这个键没有实际意义,重新映射一个fn用
caps_lock left_control
left_control f18 这个f18是为了后面映射Hyper key做准备的,你可以把自己喜欢的键位映射为f13-f19

这是我的键位映射xml,映射好自己的键位后,点击页面的Copy或者手动复制
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.local.KeyRemapping</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/bin/hidutil</string>
        <string>property</string>
        <string>--set</string>
        <string>{"UserKeyMapping":[
            {
              "HIDKeyboardModifierMappingSrc": 0x7000000E3,
              "HIDKeyboardModifierMappingDst": 0x700000029
            },
            {
              "HIDKeyboardModifierMappingSrc": 0x700000065,
              "HIDKeyboardModifierMappingDst": 0xFF00000003
            },
            {
              "HIDKeyboardModifierMappingSrc": 0x700000039,
              "HIDKeyboardModifierMappingDst": 0x7000000E0
            },
            {
              "HIDKeyboardModifierMappingSrc": 0x7000000E0,
              "HIDKeyboardModifierMappingDst": 0x70000006D
            }
        ]}</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
</dict>
</plist>
```
在启动台里面打开终端(Terminal)输入以下命令:
```bash
cd ~/Library/LaunchAgents/
vim com.local.keyRemppin.plist
p
:wq
```
如果上面命令看不懂可以使用下面这串命令
```bash
cd ~/Library/LaunchAgents/
touch com.local.keyRemppin.plist
open .
右键com.local.keyRemppin.plist文件>打开方式>其他>文本编辑>右健粘贴自己映射的xml>Command+s保存
```
粘贴好之后打开终端输入:
```bash
launchctl load com.local.keyRempping.plist
```
这样就映射好了键位,如果后续更新这个文件,必须要先取消加载再重新加载
```bash
launchctl unload com.local.keyRempping.plist
launchctl load com.local.keyRempping.plist
```
以上命令必须保证自己在`~/Library/LaunchAgents/`目录,否则会报错

**使用Hammerspoon设置Hyper key**
可以去官网下载[hammerspoon官网](http://www.hammerspoon.org/)或用HomeBrew安装:
```bash
brew cask install hammerspoon
```

在启动台里面打开终端(Terminal)输入以下命令:
```bash
cd ~/.hammerspoon/
open .
```
右健新建init.lua文件,并加入一下代码
```
require "hotkey.hotkey"
```
接着终端输入
```bash
mkdir hotkey/ && cd hotkey
open .
```

右健新建hotkey.lua,并加入一下代码
```lua
hyper = hs.hotkey.modal.new({}, 'F17')

function enterHyperMode()
  hyper:enter()
end

-- 绑定f18为hyper key
f18 = hs.hotkey.bind({}, 'F18', enterHyperMode )

-- 这里设置自己要使用的Hyper key,比如在Alfred中设置的快捷键(hotkey)为cmd+alt+shift+ctrl+i的话,下面的数组中就添加'i'、cmd+alt+shift+ctrl+y, 就添加'y',其他键位同理
-- 1 == cmd+alt+shift+ctrl+1 
-- 2 == cmd+alt+shift+ctrl+2 
-- c == cmd+alt+shift+ctrl+c 

keys = {'i','y','1','2','c'}
function hyperFun(keys)
  for i=1, #(keys) do
    hyper:bind({}, keys[i], function()
      hs.eventtap.keyStroke({'cmd','alt','shift','ctrl'}, keys[i])
      hyper.triggered = true
    end)
  end

end

hyperFun(keys)

```
如果没有生效的话,可以把Hammerspoon软件退出重新打开,或者点击小图标Reload

> 大家会用vim的话,可以直接用vim编辑

大功告成,之后大家就可以在各种应用里面设置cmd+alt+shift+ctrl+?的快捷键了,终于可以摆脱Karabiner-Elements
Ps: 
https://rakhesh.com/mac/using-hidutil-to-map-macos-keyboard-keys/

https://kalis.me/setup-hyper-key-hammerspoon-macos/