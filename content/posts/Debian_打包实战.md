---
title: Debian 打包实战
date: 2017-10-04 17:45:00
tags: [Linux]
---
> 通体参考 [Debian 新维护者手册](https://www.debian.org/doc/manuals/maint-guide/index.zh-cn.html)

# Debian 打包 vlmcsd
## 环境变量
首先设置两个环境变量，`$DEBEMAIL`和`$DEBFULLNAME`，这样大多数 Debian 维护工具就能够正确识别你用于维护软件包的姓名和电子邮件地址。
```bash
export DEBEMAIL="simonsmh@gmail.com"
export DEBFULLNAME="Simon Shi"
```
## 获取源代码和源代码包
需要根据 Debian 政策（Debian Policy）以及约定俗成的做法来调整软件包名和上游版本。参考文档 [2.6. 软件包名称和版本](
https://www.debian.org/doc/manuals/maint-guide/first.zh-cn.html#namever)。
```bash
git clone https://github.com/Wind4/vlmcsd
mv vlmcsd vlmcsd-1111
tar Jcf vlmcsd-1111.tar.xz vlmcsd-1111
```

## 进行创建初始版本
我们使用 debmake 工具将 vlmcsd 创建为 [Debian 本土软件包](https://www.debian.org/doc/manuals/maint-guide/advanced.zh-cn.html#native-dh-make)。
```bash
cd vlmcsd-1111
debmake -n
```
然后我们就可以进行初步的打包了。

## 解决软件包构建中的问题
在此次构建过程中，由于 Makefile target 的缺失，默认情况下我们只能将其打包成空包。我们需要进行一定的改进。

## install 文件
如果你的软件包需要那些标准的 make install 没有安装的文件，你可以把文件名和目标路径写入 [install 文件](https://www.debian.org/doc/manuals/maint-guide/dother.zh-cn.html#install)，它们将被 dh_install 安装。你需要首先检查要安装的文件是否有更有针对性的特定工具会对其进行安装。例如，文档应写在 docs 文件中安装，而不是这一个。
```bash
bin/vlmcs usr/bin
bin/vlmcsd usr/bin
```

## vlmcsd.service 文件
vlmcsd.service 文件会被安装为 /etc/systemd/system/vlmcsd.service ，该脚本可用于启动和停止守护进程。
```ini
[Unit]
Description=Vlmcsd (KMS Emulator in C)
After=network.target

[Service]
Type=simple
User=nobody
ExecStart=/usr/bin/vlmcsd -D -e

[Install]
WantedBy=multi-user.target
```

## vlmcsd.manpage 文件
你的程序应该有 man 手册页，如果它们没有自带则需要由你来创建。dh_make 命令创建了几个 man 手册页的模板。为每一个缺手册的命令拷贝一份模板，并妥善地编写，并且要删除未使用的模板。
```
man/vlmcs.1
man/vlmcsd.7
man/vlmcsd.8
```

## 后续
在这时，已经完成了软件包必要操作的创建了。[本软件包](https://github.com/simonsmh/vlmcsd)已经可以正常使用。