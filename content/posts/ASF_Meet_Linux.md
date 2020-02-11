---
title: 当 ArchiSteamFarm 遇上 Linux
date: 2017-02-24 19:52:00
tags: [Linux]
---
> ASF重构（.NET Framework -> .NET Core）之后不再使用mono了，尊重开发者的决定换用dotnet。  
6月26日开始重构进程，8月14日 dotnet 2.0发布。

<!-- more -->

## 在 Linux 上安装 dotnet
```
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > /etc/apt/trusted.gpg.d/microsoft.gpg
echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-stretch-prod stretch main" > /etc/apt/sources.list.d/dotnetdev.list
apt update
apt install dotnet-sdk-2.0.0
```

## 在 Linux 上安装 mono (EOL)
```
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
echo "deb http://download.mono-project.com/repo/debian jessie main" | sudo tee /etc/apt/sources.list.d/mono-official.list
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
因为有自动更新功能，所以不需要再去更新了。  
然后再将ASF的config存放到自己想要的目录。

## 小坑 on Debian Buster
由于 buster 尚在 testing，所以并没有为 buster 单独编译的 dotnet sdk，故导致了一些[喜闻乐见的问题](https://github.com/dotnet/core-setup/issues/2943)。

## 开机持续运行
```
echo 'tmux new-window /root/ArchiSteamFarm/run.sh --path=/root/.config/ASF/' >> /etc/rc.local
```
