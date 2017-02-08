---
title: 记一次gradle/android adk/android ndk/apk build尝试
date: 2016-10-18 19:09:00
tags: [Chat]
---
最近尝试在安装sdk来尝试编译一些gayhub上瞄到的程序，写个教程给自己看。

首先安装Java :cup:
```
apt install openjdk-8-jdk
```
然后安装gradle  
然而apt安装的gradle居然版本只有2.x，不愧debian
```
curl -s https://get.sdkman.io | bash
source "/root/.sdkman/bin/sdkman-init.sh #加载sdkman
sdk install gradle 3.1
```
然后处理一下sdk  
这里在apt里也有个installer，也是个坑，不要碰它
```
wget https://dl.google.com/android/android-sdk_r24.4.1-linux.tgz
tar -xf android-sdk_r24.4.1-linux.tgz
export ANDROID_HOME=$HOME/android-sdk-linux
export PATH=${PATH}:$HOME/android-sdk-linux/platform-tools:$HOME/android-sdk-linux/tools
echo 'export ANDROID_HOME=$HOME/android-sdk-linux' >> ~/.bashrc 
echo 'export PATH=${PATH}:$ANDROID_HOME/platform-tools:$ANDROID_HOME/tools' >> ~/.bashrc  #自己改了啦
```
开始使用android sdk安装sdk包
```
android update sdk -t tools -u
android update sdk -t platform-tools -u
android update sdk -t build-tools-23.0.3 -u
android update sdk -t build-tools-24.0.3 -u
android update sdk -t android-23 -u
android update sdk -t android-24 -u
android update sdk -t extra-android-m2repository -u
android update sdk -t extra-google-m2repository -u
```
最后处理一下ndk  
这里在apt里还有个installer，还是个坑，不要碰它
```
wget https://dl.google.com/android/repository/android-ndk-r13-linux-x86_64.zip
unzip android-ndk-r13-linux-x86_64.zip
rm android-ndk-r13-linux-x86_64.zip
export ANDROID_NDK_HOME=~/android-ndk-r13/
echo 'export ANDROID_NDK_HOME=~/android-ndk-r13' >> ~/.bashrc
```
gradle编译
```
gradle && gradle build
```
gradle使用时看见error
```
A problem occurred configuring project ':app'. > You have not accepted the license agreements of the following SDK components: 
[Android SDK Platform 24].
```
说明Android SDK Platform 24没有安装，android list sdk -a查一下是哪个包，然后按需安装即可  
有时候，gradle编译到一半抛出个exception（手动sticker  
vps的内存小可能会是主要因素。新写入一个swap的img文件可以给jdk留下充足空间
```
dd if=/dev/zero of=/swap bs=256M count=4
mkswap /swap
chmod 600 /swap
swapon /swap
```
注意这个在每次启动时都要手动swapon一下。 
创建keystore，生成一个名为simonsmh的key（诶嘿嘿
```
keytool -genkey -v -keystore simonsmh.key -alias simonsmh -keyalg RSA -keysize 2048 -validity 10000
```
给apk签名
```
jarsigner -keystore simonsmh.key *.apk simonsmh
```
附一个adb/fastboot
```
sudo apt install android-tools-adb android-tools-fastboot
```
安装完就使用肯定会出现No permissions，需要设置一下android.rules
```
sudo echo -e 'SUBSYSTEM=="usb", ATTRS{idVendor}=="0bb4", MODE="0666"
SUBSYSTEM=="usb", ATTRS{idVendor}=="0502", MODE="0666"
SUBSYSTEM=="usb", ATTRS{idVendor}=="12d1", MODE="0666"
SUBSYSTEM=="usb", ATTRS{idVendor}=="1004", MODE="0666"
SUBSYSTEM=="usb", ATTRS{idVendor}=="22b8", MODE="0666"
SUBSYSTEM=="usb", ATTRS{idVendor}=="04e8", MODE="0666"
SUBSYSTEM=="usb", ATTRS{idVendor}=="0fce", MODE="0666"
SUBSYSTEM=="usb", ATTRS{idVendor}=="0489", MODE="0666"
SUBSYSTEM=="usb", ATTRS{idVendor}=="18d1", SYMLINK+="android_adb", MODE="0666"
SUBSYSTEM=="usb", ATTRS{idVendor}=="04e8", MODE="0666", GROUP="plugdev"' > /etc/udev/rules.d/51-android.rules
sudo service udev restart && killall adb
```
这样就能看见设备了。