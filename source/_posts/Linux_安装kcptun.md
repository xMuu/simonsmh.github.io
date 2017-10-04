---
title: Linux安装kcptun
date: 2016-09-04 11:04:00
tags: [Linux]
---
> [kcptun](https://github.com/xtaci/kcptun/releases)

## kcptun 安装
```bash
wget https://github.com/xtaci/kcptun/releases/download/v20160902/kcptun-linux-amd64-20160902.tar.gz
tar -xf kcptun-linux-amd64-20160902.tar.gz
mv server_linux_amd64 /usr/bin/kcptun 
chmod 755 /usr/bin/kcptun 
rm -rf client_linux_amd64 kcptun-linux-amd64-20160902.tar.gz
```
使用
```bash
kcptun -l :8399 -t 127.0.0.1:8388 --crypt none --nocomp --mtu 1492 --sndwnd 2048 --rcvwnd 2048 --mode default -datashard 10 -parityshard 3 --dscp 46 &
```
ss-android端参数
```bash
--crypt none --nocomp --mtu 1492 --sndwnd 64 --rcvwnd 64 --mode default -datashard 10 -parityshard 3 --dscp 46
```
测试手动参数  
client
```bash
-mode manual -nodelay 0 -resend 0 -nc 1 -interval 40 -dscp 46 -nocomp -mtu 1492 -crypt salsa20 -datashard 70 -parityshard 30
```
服务端
```bash
-mode manual -nodelay 0 -resend 0 -nc 1 -interval 40 -dscp 46 -nocomp -mtu 1492 -crypt salsa20 -datashard 70 -parityshard 30
```