---
title: Debian编译自定义内核
date: 2016-02-27 12:48:00
tags: [Linux]
---

### 内核修改提速
```
sed -i s/'tp->snd_cwnd = min(tp->snd_cwnd'/'tp->snd_cwnd = max(tp->snd_cwnd'/ net/ipv4/tcp_input.c
sed -i s/'define TCP_INIT_CWND  10'/'define TCP_INIT_CWND   96'/ include/net/tcp.h
sed -i s/'define TCP_RTO_MIN    ((unsigned)(HZ\/5))'/'define TCP_RTO_MIN    ((unsigned)(HZ\/25))'/ include/net/tcp.h
```

### 编译内核
```
wget https://www.kernel.org/pub/linux/kernel/v4.x/linux-4.4.3.tar.gz
tar -xf linux-4.4.3.tar.gz
cd linux-4.4.3
apt-get install kernel-package
make oldconfig
make menuconfig
make-kpkg clean
make-kpkg --initrd kernel-image -j3
cd ..
dpkg -i linux-*.deb
```
测试完毕，仅hybla有奇效。