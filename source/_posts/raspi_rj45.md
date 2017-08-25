---
title: 树莓派3做RJ45网卡接无线网（误
date: 2017-06-17 00:03:00
tags: [Linux]
---
## 丢配置 
```
[Match]
Name=eth0

[Network]
Address=10.0.1.1/24
DHCPServer=yes
IPForward=yes
IPMasquerade=yes

[DHCPServer]
DNS=119.29.29.29
EmitDNS=yes
EmitRouter=yes
```
这个是 systemd-networkd 在/etc/systemd/network/里的eth0配置文件，将树莓派当作网关处理数据包。  
其余可以随意按照正常方式接入无线网络（NetworkManager），也可以搭建dnsmasq做dhcp/dns服务器，同时赞美systemd。
