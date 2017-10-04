---
title: 为 wndr4300 编译 openwrt / lede
date: 2016-03-25 17:37:00
tags: [openwrt]
---
# 为 wndr4300 编译 lede
## 准备工作
```bash
sudo apt-get install subversion build-essential libncurses5-dev zlib1g-dev gawk git ccache gettext libssl-dev xsltproc
cd
git clone git://git.openwrt.org/15.05/openwrt.git
cd openwrt
```

## 源代码同步构建 (out of dated)
```bash
git pull
#git checkout 15.05.01 #检查15.05.01稳定分支
./scripts/feeds update -a
./scripts/feeds install luci
wget https://downloads.openwrt.org/chaos_calmer/15.05.01/ar71xx/nand/config.diff
sed -i  s/'CONFIG_TARGET_ar71xx_nand_R6100=y'/'CONFIG_TARGET_ar71xx_nand_WNDR4300=y'/ config.diff
sed -i  s/'23552k(ubi),25600k@0x6c0000(firmware)'/'120832k(ubi),122880k@0x6c0000(firmware)'/ target/linux/ar71xx/image/Makefile
cat config.diff >> .config
make defconfig
make -j #多线程
```

## 使用 openwrt image builder 快速构建
```bash
wget https://downloads.lede-project.org/releases/17.01.2/targets/ar71xx/nand/lede-imagebuilder-17.01.2-ar71xx-nand.Linux-x86_64.tar.xz
tar -xf lede-imagebuilder-17.01.2-ar71xx-nand.Linux-x86_64.tar.xz
cd lede-imagebuilder-17.01.2-ar71xx-nand.Linux-x86_64
make info #查看默认软件包和硬件支持列表
sed -i  s/'23552k(ubi),25600k@0x6c0000(firmware)'/'120832k(ubi),122880k@0x6c0000(firmware)'/ target/linux/ar71xx/image/legacy.mk
make image PROFILE="WNDR4300V1" -j #指定机型+多线程
```

## 常用软件包
```bash
opkg install wget ca-bundle ca-certificates luci-i18n-base-file-zh-cn luci-i18n-firewall-zh-cn iptables-mod-tproxy ip shadowsocks-libev luci-app-shadowsocks ChinaDNS luci-app-chinadns pdnsd luci-app-pdnsd luci-app-samba luci-i18n-samba-zh-cn block-mount kmod-usb-storage-extras kmod-fs-ntfs kmod-fs-vfat kmod-fs-exfat
```
