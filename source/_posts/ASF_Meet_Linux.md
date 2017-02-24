---
title: 当 ArchiSteamFarm 遇上 Linux
date: 2017-02-24 19:52:00
tags: [Linux]
---
## 在 Linux 上安装 ArchiSteamFarm
```
apt install mono-complete
git clone git@github.com:JustArchi/ArchiSteamFarm.git
```
## 编译exe
```
cd ArchiSteamFarm
./cc.sh
```
因为有自动更新功能，所以不需要再去更新了
然后再将ASF的config存放到自己想要的目录。
## 开机持续运行
```
echo 'screen -dm /root/ArchiSteamFarm/run.sh --path=/root/.config/ASF/ &' >> /etc/rc.local
```
