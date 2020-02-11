---
title: 玩耍 TicWatch C2
date: 2019-03-28 09:34:00
tags: [杂谈]
---
之前就种草很久的 TicWatch 系列，由于最近的新品 C2 的上市打算购买一个来体验一下 Wear OS 的体验，而且正好小米手环2坏掉了（迫真），于是就从京东预定了一个 TicWatch C2 进行一下评测。

## 与 TicWatch C2 的相遇
京东预定的 TicWatch C2 在首发价格的基础上减去100块，实际到手价1k2，虽然有点小贵，但是外观和早些发售的 TicWatch Pro 相比还是非常好看的。外观简洁，整机金属外壳，皮质表带，给人感觉非常坚实可靠。

TicWatch C2 的配置是高通210，512M的Ram，2G可用空间，只能说勉强够用，毕竟 Wear OS 没有开太多后台的需求。360*360分辨率的oled，315dpi，屏幕显示效果一般，但能满足显示需求，强光下的可见程度还算优秀。

特性方面，TicWatch C2 支持 NFC，系统预装支付宝，支持24小时心率监测，支持 TicMotion 运动监测，IP68级别防水可以游泳。

相比 TicWatch Pro，其缺少自适应光线，LED副屏和扬声器，还是较为令人困扰的。

## 糟心的 Wear OS for China
Wear OS 中国版的生态用两个字形容，**糟糕**。

Wear OS 中国版 是 Google 与出门问问合作制作并推广的，对于出门问问厂商来说，这是一次千载难逢的好机会，解决了 TicWatch 长期以来的系统适配问题，而对于其他厂商来说则是一声噩耗，出门问问单方霸占了重要的流量入口：语音助手，其他厂商无法绕开这一限制，使得出门问问的一手好牌打得稀烂，软件生态几乎为0。

系统预装 Wear OS 兼容软件：滴滴出行，搜狗地图，支付宝，网易云音乐

我额外安装的 Wear OS 兼容软件：微信

在使用了一周的 Wear OS for China 2.0 之后，我收到了 Wear OS for China H 2.2 更新，底层更新到Pie（PWDS.190212.008），体验较之前更进一步，支持低电量自动开启省电模式，解决了一大痛点，整体设计风格更偏向 Material Design 2。

## 改变现有的系统固件
网上看到了其他人在为 TicWatch Pro 制作[ROM](https://forum.xda-developers.com/smartwatch/other-smartwatches/rom-kernel-t3821013/page136)，非常好奇，而向其询问了一些事项，了解了 TicWatch 解锁刷机的大致步骤，底座触点就是 USB。

### 适配 TWRP
折腾花了一周半，主要是没有能够启动的内核。跟开发者聊了一下，好歹是个能启动起来的内核了，启动之后再把表里的内核dd出来。

[制作好的 TWRP 已经提交 TeamWin 官网](https://twrp.me/mobvoi/mobvoiticwatchc2.html)

安装好 TWRP 后，首先得备份所有的 OTA 相关的文件，大概有十个分区，否则难以回到官方环境下继续享受系统更新。OTA 的检查还是非常严格的。

### 使用 Magisk 模块进行系统文件更替
根据[出门问问论坛](https://bbs.chumenwenwen.com/forum.php?mod=viewthread&tid=412586&extra=page%3D1%26filter%3Dtypeid%26typeid%3D167)上的交流中谈到，如果去除 `cn_google_feature.xml` 就可以使用国际版的系统，体验几乎一致，而修改系统显然是我不能接受的，于是我使用常用的 Magisk 实现。

由于 Magisk 的特性，如果有备份 boot.img，此模块就将不会影响 OTA 的更新。

[TicWatch CN Google-ify (msm8909-pie)](https://github.com/simonsmh/ticwatch-googleify)

此外为了禁用出门问问语音助手以正常使用 Google Assistant，请在adb中执行以下命令：

```
pm uninstall --user 0 com.mobvoi.ticwear.sidewearvoicesearch
```

## 仍无他用的 Wear OS Global
安装 Google Play 后，看着屈指可数的几个应用，我才发现，Wear OS 生态的差距并不体现在国内，而是全球的问题。

Google 第一方的程序设计精美，动效优异，但是迟钝，210明显负担不起这样的负载。Google 地图 体验不错，定位偏移很遗憾；Google Fit 不如 TicWatch 自家的应用，何况还支持数据同步；Telegram 设计尚可，但真有人想在手表上聊IM吗？其余的应用没有过多体验，少用几款省省电。

## 总结
请趁早打消购买 Wear OS 设备的念头。

当然，如果你有闲钱，Wear OS 设备还是可以支付的起的话，不如购入体验一下，秒杀一众伪智能表产品还是绰绰有余。

Wear OS 设备的性能限制和电量控制注定了它只是个手表而已，缺少杀手级的应用不会使得表成为一块破铜烂铁，但是总会使人感觉少了些什么，尤其是很少使用语音助手的我，很少体育锻炼的我，很少脱离手机的我，是否真的需要这样一部可穿戴设备？我觉得不。
