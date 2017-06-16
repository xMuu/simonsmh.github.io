---
title: 树莓派3 与 Arch linux ARM
date: 2017-06-17 00:03:00
tags: [Linux]
---
刷写镜像参考[ArchLinuxARM文档](https://archlinuxarm.org/platforms/armv8/broadcom/raspberry-pi-3)  
先换源，然后安装些软件包
```
pacman -Syu xorg xorg-xinit xf86-video-fbdev xorg-xrefresh make gcc git automake autoconf fakeroot pkg-config libtool xfce4 xfce4-goodies lightdm lightdm-gtk-greeter xarchiver vim tmux ffmpeg chromium sudo iw wireless_tools wpa_supplicant noto-fonts noto-fonts-cjk noto-fonts-emoji ttf-roboto networkmanager network-manager-applet rsync patch packer
systemctl enable lightdm NetworkManager
```
重启，就可以在桌面环境使用普通用户了。  
使用AUR装俩主题
```
packer -S paper-icon-theme-git adapta-gtk-theme-git
```
中文
```
locale-gen
echo LANG=zh_CN.UTF-8 > /etc/locale.conf
```
Arch是我接触过的第三个Linux发行版，虽然被称为文档最详细的邪教，ARM平台不受官方支持实在令人苦恼。
今年年底Arch平台即将放弃32位支持，而树莓派3的AArch64架构驱动仍然迟迟未来，会有多少教徒坚守社区呢？

另外拔电源等非正常关机后，开机必定Kernel Panic一次，目前还没有找到好的解决措施。
