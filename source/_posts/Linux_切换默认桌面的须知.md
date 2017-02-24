---
title: Linux切换默认桌面的须知
date: 2016-12-31 19:30:00
tags: [Linux]
---
vps上准备切换到xfce桌面环境，事先做好了准备工作，但是在最后一步卡住了。  
xsession默认选择的是lxde，我想尽办法搜索相关的信息，包括xinit，startx的配置文件，lightdm的配置方法，都在Arch wiki上找到了，学到了很多知识，但并没有什么卵用。
```
update-alternatives --config x-session-manager
```
这个命令可以解决问题。  
重新启动，发现系统中早已完成了桌面环境的启动，而无法成功实现切换
```
root@simonsmh:~# startxfce4
/usr/bin/startxfce4: Starting X server

(EE)
Fatal server error:
(EE) Server is already active for display 0
        If this server is no longer running, remove /tmp/.X0-lock
        and start again.
(EE)
(EE)
Please consult the The X.Org Foundation support
         at http://wiki.x.org
 for help.
(EE)
^CInvalid MIT-MAGIC-COOKIE-1 keyxinit: giving up
xinit: unable to connect to X server: Resource temporarily unavailable
xinit: unexpected signal 2
root@simonsmh:~# startlxde
** Message: main.vala:102: Session is LXDE
** Message: main.vala:103: DE is LXDE

(lxsession:1916): Gtk-WARNING **: cannot open display:
root@simonsmh:~#
```
由此可知，一定是某项开机启动的x视窗服务未更改默认的session  
然而，寻找到x11下的session，删除lxde的session后，发现并未奏效。  
而且，寻找到lightdm下有默认登录的session配置，却未启用。  
startx xinitrc的配置也如同平常，vnc配置的选项依旧冲突，于是只可能是x服务本身的问题。  
再然后，重新配置x-session-manager，问题解决。

顺便，unity桌面史上最差，kde高特技低效率，lxde丑哭，xfce最棒不服来辩。  
主题使用Adapta，图标使用Paper，效果很赞。