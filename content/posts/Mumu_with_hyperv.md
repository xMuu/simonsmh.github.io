---
title: "Mumu 模拟器 & WSL2，双倍的快乐"
date: 2020-05-21T23:09:18+08:00
tags: [杂谈]
---

> 第一次有了最香的 WSL2，有了好用的 Mumu 模拟器，两件快乐事情重合在一起，而这两份快乐，又给我带来更多的快乐。得到的，本该是像梦境一般幸福的时间……

## 国产手游模拟器与 Hyper-V 之争
Mumu 是一款商业化的 Android X86 手游模拟器，其运行手游的效率相对较高，体验尚佳，我曾经常使用，但是由于其无法与 Hyper-V 共存，便一直在寻找替代品。事实上，基本所有的模拟器都是基于开源的 Virtualbox 进行魔改提升效率，实现 Direct3D 11 渲染，完善用户体验，但是其基本的需求都是使用魔改 Virtualbox 自己的后端实现而达到的，自然从原理上就无法与 Hyper-V 兼容。

这在旧时 Windows 时期，其实是件大快人心的好事，毕竟 Hyper-V 其运行效率难以与传统的虚拟机平台 Vmware、Virtualbox 竞争，也被吐过不少口水。不巧的是 Hyper-V 近年来用处是越来越多了，从一键开发虚拟环境搭建，到 Container，到 Windows Sandbox，一直到 WSL2 宣布使用 Hyper-V+Linux Kernel 方案替代 WSL1 的 Windows NT API 方案，日常的生活是越来越绕不开 Hyper-V 了。当然，强制让 Virtualbox 使用 Hyper-V 后端的解决方案也有，但是性能就比较难以令人接受，感兴趣可以通过修改 Genymotion 附带的 Virtualbox 配置来尝试（scrcpy 在高分屏上可能有性能问题）。

## 写作 Nebula，读作 Windroy?
2020 年初，Mumu 手游助手测试版于网易 Mumu 官网推出，其与原版 Mumu 模拟器最不相同的地方就是在于其在原版引擎 Nemu 的基础上添加了一个新引擎 Nebula，其原理上就不同于传统的 Virtualbox，它不是跑在 Linux Kernel 之上，而是直接跑在 Windows Kernel 上，换句话说，其实现了 Windows API 与 Android API 的桥接与翻译。第一眼看见它时，我第一个想到的就是 6 年之前的 Bluestacks 和 [Windroy](http://web.archive.org/web/20141006160302/http://www.windroy.cn/) 这两款特立独行的 Windows 平台 Android X86 兼容软件，依赖其出色的 [Dalvik](https://source.android.com/devices/tech/dalvik) 性能而脱颖而出，但可惜的是这两款软件后来放弃了这个方向，带领行业纷纷转用虚拟机实现。

![image.png](https://i.loli.net/2020/05/22/UrZSAH4y6aquojv.png)

下载试用 Nebula 星云引擎，果然因为其原理的不同，其启动速度飞快，反应迅速，界面也非常“熟悉”，是“家”一般的感觉——最后一版支持 Dalvik 运行时的 Android 4.4 KitKat，而与之相对的是 Nemu 标准引擎带来的 Android 6.0 Marshmallow。由于 Hyper-V 的冲突和限制，Nemu 引擎启动时就会触发 Windows 蓝屏，而 Nebula 就可以毫无压力的进行使用，且游戏性能确实优于 Nemu，运行游戏极少掉帧。

![image.png](https://i.loli.net/2020/05/22/kwPbA1gVMzGCK7n.png)

相对运行完整虚拟机来说，在运行舟游等低负载压力的 Unity 游戏情况下显卡占用还是比较低的，体验可以说是相当不错了。

![image.png](https://i.loli.net/2020/05/22/Th1RPyz5iLgSvBr.png)

虚拟机虽然跑不了 adbd，没有 Root，但是自带 Busybox 提供了一个 Root Shell 接口在
`.\MuMu\mumu3\Engine\NEBULA\NEBULA-<version>-x86\sh.bat`
```shell
nebula\nebula.exe --cooked-mode --rootdir nebula/fs_static --session-id *** --dynamicdir *** /system/bin/sh 
```

其中 `dynamicdir` 就是系统之外数据的部分，似乎每个软件都应当有独立的空间，而引擎在使用的时候按需挂载。

Mumu 限制了通过 GUI 为 Nebula 安装自己的软件包，但是可以通过这个接口使用命令行安装：
```shell
pm install -f *.apk
```
当然，因为其是 Dalvik，没有 dex2oat 的步骤， 直接拷贝至 `/data/app` 后重启也是可以的。

我也尝试着拷贝了一份台服公主连结的 APK，不管是游戏引继还是资源包下载安装都没有遇到什么障碍，由于模拟器可以锁渲染 120 帧，很多地方都比手机要更加流畅。

然而，在 2020 年的今天，寻找 MinSDK<=19 的软件早就已经不是件容易事，日新月异的的 API 吸引着更多的应用开发者逐步抛弃低市占率的平台已是既成事实，但好在游戏行业大多不受其限制，毕竟更多的用户往往意味着更多的收入，这也是为什么如此低的 API 版本依然使 Nebula 对使用 Hyper-V 的手游玩家来说值得一用。

## 结语与展望
Windows 的子系统和容器的发展在近些年开始对消费者产生了难以抵抗的吸引力，但由于其与开源开放的 Linux 平台容器技术不同，以及 Android 平台本身就基于 Linux 内核开发，类似 Anbox 的容器应用曾经几乎不可能在 Windows 平台出现。今年的微软 Build 大会之上，微软首次展示了 WSL 与 GPU 的交互，展示了 WSL 平台的 GUI 应用，这或许意味着 Anbox 也可以通过 WSL 的 GPU 进行渲染，而不再需要额外的模拟器（当然，效率究竟几何仍需打个问号），又或许将来某一天 Project Astoria 就能以这种身份复活在 Windows Shell Experience 中。
