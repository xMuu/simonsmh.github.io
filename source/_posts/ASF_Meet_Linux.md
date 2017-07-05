---
title: 当 ArchiSteamFarm 遇上 Linux
date: 2017-02-24 19:52:00
tags: [Linux]
---
ASF重构（.NET Framework -> .NET Core）之后不再使用mono了，尊重开发者的决定换用dotnet。
6月26日开始重构进程，目前项目暂时不可用
## 在 Linux 上安装 dotnet
```
echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/dotnet/ jessie main" > /etc/apt/sources.list.d/dotnetdev.list
apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
apt update
apt install dotnet-sdk-2.0.0-preview1-002111 #(Not found)
# dotnet sdk 依然处于活跃开发期，版本不同严重影响编译
```
## 在 Linux 上安装 mono (EOL)
```
apt update
apt install mono-complete
```
## 拖 ASF 源码
```
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
