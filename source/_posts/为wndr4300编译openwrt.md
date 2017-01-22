---
title: 为 wndr4300 编译 openwrt
date: 2016-03-25 17:37:00
tags: [openwrt]
---

## 为 wndr4300 编译 openwrt

### 准备工作
```
sudo apt-get install subversion build-essential libncurses5-dev zlib1g-dev gawk git ccache gettext libssl-dev xsltproc
cd
git clone git://git.openwrt.org/15.05/openwrt.git
cd openwrt
```

### 源代码同步构建
```
git pull
#git checkout 15.05.01 #检查15.05.01稳定分支
./scripts/feeds update -a
./scripts/feeds install luci
wget https://downloads.openwrt.org/chaos_calmer/15.05.01/ar71xx/nand/config.diff
sed -i  s/'CONFIG_TARGET_ar71xx_nand_R6100=y'/'CONFIG_TARGET_ar71xx_nand_WNDR4300=y'/ config.diff
sed -i  s/'23552k(ubi),25600k@0x6c0000(firmware)'/'120832k(ubi),122880k@0x6c0000(firmware)'/ target/linux/ar71xx/image/Makefile
cat config.diff >> .config
make defconfig
make -j2 #双线程
```

### 使用 openwrt image builder 快速构建
```
wget https://downloads.openwrt.org/chaos_calmer/15.05.1/ar71xx/nand/OpenWrt-ImageBuilder-15.05.1-ar71xx-nand.Linux-x86_64.tar.bz2
tar -xf OpenWrt-ImageBuilder*.tar.bz2
cd Openwrt-ImageBuilder*
make info #查看默认软件包和硬件支持列表
sed -i  s/'23552k(ubi),25600k@0x6c0000(firmware)'/'120832k(ubi),122880k@0x6c0000(firmware)'/ target/linux/ar71xx/image/Makefile
make image PROFILE="WNDR4300" -j2 #指定机型+双线程
```

### 常用软件包
```
opkg install nano luci luci-app-commands luci-i18n-base-zh-cn luci-i18n-commands-zh-cn luci-i18n-firewall-zh-cn luci-app-samba luci-i18n-samba-zh-cn block-mount block-hotplug kmod-usb-storage kmod-usb-storage-extras kmod-fs-vfat kmod-fs-ntfs kmod-fs-exfat kmod-scsi-core kmod-scsi-generic kmod-nls-cp437 kmod-nls-iso8859-1 e2fsprogs shadowsocks-libev luci-app-shadowsocks
```