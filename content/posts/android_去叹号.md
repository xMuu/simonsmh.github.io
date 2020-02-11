---
title: android 去叹号
date: 2016-12-08 14:42:00
tags: [杂谈]
---
> 从Android 5.0 (Lollipop)开始，安卓为了检测各种需要登录的Wifi服务，提供了captive_portal_detection_enabled和captive_portal_server这两个设置，分别作为检测的开关和服务器。

## 去叹号
```bash
settings delete global captive_portal_server
settings delete global captive_portal_http_url
settings delete global captive_portal_https_url
settings put global captive_portal_https_url https://developers.google.cn/generate_204
```
