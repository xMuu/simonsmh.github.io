---
title: Debian8 安装锐速实录
date: 2016-05-26 22:18:00
tags: [Linux]
---

### 安装3.12-1-amd64内核
完全采用Debian8发行版，感谢[91yun](https://github.com/91yun/serverspeeder/)的脚本。
由于Debian官方不再支持3.12lts内核，只能通过snapshot做变通的解决方法。
```
wget http://snapshot.debian.org/archive/debian/20140310T221406Z/pool/main/l/linux/linux-image-3.12-1-amd64_3.12.9-1_amd64.deb
wget http://snapshot.debian.org/archive/debian/20140310T221406Z/pool/main/l/linux/linux-headers-3.12-1-common_3.12.9-1_amd64.deb
dpkg -i *.deb
```
安装完成重启时请自行选择内核，删除原内核
```
apt remove linux-image-amd64
```
然后用脚本安装锐速
```
wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh && bash serverspeeder-all.sh
```

### 卸载
````
chattr -i /serverspeeder/etc/apx* && /serverspeeder/bin/serverSpeeder.sh uninstall -f
````
