---
title: Debian+Nginx配置Let’s Encrypt
date: 2016-02-06 17:46:00
tags: [Linux]
---
### debian安装Let’s Encrypt

```
apt update
apt install letsencrypt
letsencrypt --duplicate certonly --standalone --email simonsmh@gmail.com -d simonsmh.cc -d blog.simonsmh.cc -d tieba.simonsmh.cc
```
### nginx需要的证书位置  

```
#certificate
/etc/letsencrypt/live/simonsmh.tk/fullchain.pem
#privatekey
/etc/letsencrypt/live/simonsmh.tk/privkey.pem
```
### nginx配置示例

```
listen 443;
...
ssl on;
ssl_certificate /etc/letsencrypt/live/simonsmh.tk/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/simonsmh.tk/privkey.pem;
ssl_session_timeout 10m;
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers ALL:!aNULL:!ADH:!eNULL:!LOW:!EXP:RC4+RSA:+HIGH:+MEDIUM;
ssl_session_cache builtin:1000 shared:SSL:10m;
...
```
