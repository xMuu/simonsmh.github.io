---
title: openwrt 网段互访和防火墙设置
date: 2016-05-08 15:55:00
tags: [openwrt]
---
## openwrt 网段互相访问
网络-静态路由
A路由ip wan 192.168.1.10 lan 10.0.0.1
wan 10.0.1.0/24 255.255.255.0 192.168.1.11
B路由ip wan 192.168.1.11 lan 10.0.1.1
wan 10.0.0.0/24 255.255.255.0 192.168.1.10

## openwrt 开放80/22端口远程访问
uci set uhttpd.main.rfc1918_filter=0
uci commit uhttpd
网络-防火墙-通信规则
打开路由器端口:
80 tcp 80 添加
22 tcp 22 添加