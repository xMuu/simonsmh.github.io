---
title: openwrt无线中继
date: 2017-02-24 19:46:00
tags: [openwrt]
---
## openwrt无线中继
1. 先在 网络-WiFi 中 Add 一个 Client, 进行链接先前路由.
2. Interfaces - LAN 中 creates a bridge.
3. 将所有 Ethernet Adapter and Wireless Network 勾选到 LAN 上.
4. 自由添加 Wifi Master, 关闭防火墙.
5. 手动编辑 config, 开启 Client 40mhz.
