title: cubieboard 无线网络配置
date: 2013-07-28 00:15:48
categories: 
tags: cubieboard2
---

## 准备

1. 先确保已经装好了无线网卡的驱动，我用的是LUbuntu，已经集成了8188u的芯片驱动了
2. 使用ifconfig查看无线网卡的配置，我的是wlan0，如果看不到无线的配置，使用iwconfig查看

<!--more-->

## 配置网卡自动获取IP

打开`/etc/network/interfaces`, 添加如下配置

```
auto wlan0
iface wlan0 inet dhcp
wpa-essid "要连接的无线网络名称"
wpa-psk "无线网络的连接密码"
```

如果你的无线网卡是wlan1，则将配置中的wlan0修改为wlan1.

配置完成后，通过`service networking restart`重启网络，使用`ifconfig`查看ip是否已经分配好了。