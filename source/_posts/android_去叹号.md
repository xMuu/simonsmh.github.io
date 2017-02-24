---
title: android 去叹号
date: 2016-12-08 14:42:00
tags: [Chat]
---
去叹号
```
settings delete global captive_portal_server
settings delete global captive_portal_http_url
settings delete global captive_portal_https_url
settings put global captive_portal_https_url https://developers.google.cn/generate_204
```