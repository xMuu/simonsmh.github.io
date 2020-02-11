---
title: Hyper-v Gen2 UEFI 与 Arch Linux
date: 2017-10-04 17:21:00
tags: [Linux,Windows]
---
> WSL 不够用，装个虚拟机看看

# 在 Hyper-v 中以 UEFI 引导安装 Arch Linux

## 检查网络链接
ping 一下外网。
```
ping -c 3 archlinux.org
```
## 分区
```
gdisk /dev/sda
```
然后进去 gdisk 操作。

`o # GPT 分区表`  
UEFI 需要单独 FAT32 分区，这里跟 boot 分在一起。  
`n` `1` 回车 `+300M` `ef00 #boot`  
`n` `2` 回车 回车 `8300 #root`  
`w` 写入配置，重启即可。  
```
reboot
mkfs.vfat -F32 /dev/sda1
mkfs.ext4 /dev/sda2
```
挂载分区。
```
mount /dev/sda2 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
```
## 安装基本组件
```
pacstrap /mnt base base-devel
genfstab -U /mnt >> /mnt/etc/fstab
```
安装 UEFI 引导。
```
bootctl --path=/mnt/boot install
cp /mnt/usr/share/systemd/bootctl/arch.conf /mnt/boot/loader/entries/
blkid -s PARTUUID -o value /dev/sda1 #75a1a751-2c92-457d-b97b-75abc3f9ebbb
```
进行设置。
```
arch-chroot /mnt
echo XPS15 > /etc/hostname
passwd
```

## 后续
然后电脑更新 16299 爆炸了，没法加载 hyper-v 驱动，就没法继续搞事了。