---
title: 在 Arch Linux 上编译 Lineage 15.1 和 TWRP
date: 2018-01-29 15:00:00
tags: [chat]
---
> 在 Arch Linux 上编译 Lineage 15.1 和 TWRP

## 安装编译依赖
一个AUR空包搞定
```bash
yaourt -S lineageos-devel jdk8-openjdk
```

## 新建 Lineage 源码库
```bash
mkdir -p ~/android/lineage
cd ~/android/lineage
repo init -u https://github.com/LineageOS/android.git -b staging/lineage-15.1
mkdir -p .repo/local_manifests
repo sync -c --force-sync -j16
```

## 拖机型匹配的 device tree
使用`breakfast`就可以寻找所需要的文件，但是不如自己控制来的方便。  
我使用的 oneplus3 的 lineageos 15.1 的 roomservice 如下：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
<project name="TheMuppets/proprietary_vendor_oneplus" path="vendor/oneplus" remote="github" revision="lineage-15.1"/>
<project name="dianlujitao/android_kernel_oneplus_msm8996" path="kernel/oneplus/msm8996" remote="github"/>
<project name="LineageOS/android_device_oneplus_oneplus3" path="device/oneplus/oneplus3" remote="github" revision="lineage-15.1"/>
<project name="LineageOS/android_device_oppo_common" path="device/oppo/common" remote="github" revision="lineage-15.1"/>
<project name="LineageOS/android_device_qcom_common" path="device/qcom/common" remote="github"/>
<project name="LineageOS/android_packages_resources_devicesettings" path="packages/resources/devicesettings" remote="github"/>
</manifest>
```
我使用的 oneplus3 的 omni 8.1 的 roomservice 如下：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--Please do not manually edit this file-->
<manifest>
  <project name="TheMuppets/proprietary_vendor_oneplus" path="vendor/oneplus" remote="github" revision="lineage-15.1" />
  <project name="android_kernel_oneplus_msm8996" path="kernel/oneplus/msm8996" remote="omnirom" revision="android-8.1" />
  <project name="dianlujitao/android_device_oneplus_oneplus3" path="device/oneplus/oneplus3" remote="github" revision="android-8.1" />
  <!--<project name="android_device_oneplus_oneplus3" path="device/oneplus/oneplus3" remote="omnirom" revision="android-8.1" />-->
  <!--<project name="LineageOS/android_vendor_qcom_opensource_cryptfs_hw" path="vendor/qcom/opensource/cryptfs_hw" remote="github" revision="staging/lineage-15.1" />-->
  <project name="TeamWin/android_device_qcom_common" path="device/qcom/common" remote="github" revision="android-8.0" />
</manifest>
```
将此文件存放在`.repo/local_manifests/roomservice.xml`即可同步所需文件。

## 解决编译问题
### 不能使用 python3
```bash
ln -s /usr/bin/python2 ~/bin/python
ln -s /usr/bin/python2-config ~/bin/python-config
PATH=~/bin:$PATH
```
### 中文环境问题
```bash
export 'ANDROID_JACK_VM_ARGS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation"' >> ~/.bashrc
LANG=C
LC_CTYPE=C
```
### ccache 缓存
```bash
echo 'export USE_CCACHE=1
export CCACHE_COMPRESS=1' >> ~/.bashrc
ccache -M 30G
```
### 在 gerrit 选取自定义 commit/tag
```bash
. build/envsetup.sh
repopick -t oreo-mr1-perf-profiles
```
### 清理编译环境
```bash
make clean
rm -rf out
```
## 开始编译
LineageOS
```bash
PATH=~/bin:$PATH && LANG=C && LC_CTYPE=C
. build/envsetup.sh
brunch oneplus3
```
TWRP
```bash
PATH=~/bin:$PATH && LANG=C && LC_CTYPE=C
. build/envsetup.sh
lunch omni_oneplus3-userdebug
mka recoveryimg
#brunch oneplus3
```
### 自定义编译 target
```bash
add_lunch_combo lineage_oneplus3-release
add_lunch_combo omni_oneplus3-userdebug
brunch
```
