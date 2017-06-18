---
title: windows & linux 双系统时间冲突
date: 2017-06-16 11:55:54
categories: linux学习笔记
tags: [系统时间，同步]
---
> 前段时间，陪伴我三年的 HP ENVY 15 突然开不了机。拿去售后检查，结果主板上很多元件都已经烧掉了，究其原因，就是两年前被我那可爱的小妹妹泼了可乐，<!--more-->两年后才出事我不知该庆幸还是难过。
> 
> 无奈买了一台二手的 T430 ，换上旧机器幸存下来的 SSD ，用着还蛮顺畅。  
> 
> 新电脑到手，首先就是装系统。一直都想用 win + linux 的双系统，可是上一台机器因为 UEFI 的玄学问题，实现起来特别麻烦。这次终于有机会实现对我来说最完美的系统配方。  

装好 mint 和 win10 的双系统之后，发现切换系统的时候，系统显示的时间会出错，而且正好都是差了 8 个小时。上网搜了一下才知道，是因为 linux 和 windows 默认的时间管理方式不同，所以切换系统的时时候会出现两个系统的时间不同步的问题。

- mint 默认把 BIOS 时间当作 GMT+0（世界标准时）时间，操作系统中显示的时间是经过换算得到的。当我们把 mint 的系统时区设为中国（GMT+8）的时候，系统上显示的时间就变成 BIOS 时间 + 8 小时。例如，当前的 BIOS 时间为 0 时，系统时区为中国（GMT+8）那么系统显示的时间就是 0 + 8 = 上午8时。

- windows 默认将 BIOS 时间当作本地时间,操作系统上显示的时间和 BIOS 中的时间是一样的。

当我们进入操作系统发现时间不正确，打开时间同步，把当前系统设置成正确的时间之后，也改变了电脑的 BIOS 时间。所以在切换系统之后会发现两个系统的时间又变得不一样了。

## 解决方法
- 让 win10 启用 UTC 使用 linux 的时间管理方式。
 1. 打开运行窗口 ( Win + R ) ，然后输入 <code>regedit</code> 启动注册表编辑器，并且找到以下目录  
```
HKEY_LOCAL_MACHINE/SYSTEM/CurrentControlSet/Control/TimeZoneInformation/
```
 2. 添加一项类行为 <code>REG_DWORD</code> 的键值，命名为 <code>RealTimeUniversal</code> ，值为 <code>1</code> 然后重启后时间即恢复正常。

- 在 mint 下关闭 UTC 。
 1. 首先确定你的硬件时钟是否设置为本地时间，<code>Ctrl + Alt + T</code> 打开终端，输入以下命令
```
timedatectl | grep local
```
 2. 将硬件时钟设置为本地时间
```
timedatectl set-local-rtc 1
# timedatectl set-local-rtc 0 为打开 UTC 时间
```

