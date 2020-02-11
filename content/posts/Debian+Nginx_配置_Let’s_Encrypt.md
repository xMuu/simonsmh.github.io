---
title: Debian + Nginx 配置 Let’s Encrypt
date: 2016-02-06 17:46:00
tags: [Linux]
---
> Let’s Encrypt 部署过程

## Debian 安装 Let’s Encrypt (OUT)
```bash
apt update
apt install letsencrypt
letsencrypt --duplicate certonly --standalone --email simonsmh@gmail.com -d simonsmh.cc -d blog.simonsmh.cc -d tieba.simonsmh.cc
```

## Debian 安装 acme.sh
见官方[说明](https://github.com/Neilpang/acme.sh/wiki/%E8%AF%B4%E6%98%8E)
```bash
curl  https://get.acme.sh | sh
acme.sh --issue -d simonsmh.cc -d app.simonsmh.ccc-d url.simonsmh.cc -d tieba.simonsmh.cc --dns dns_cf --installcert --fullchain-file /etc/nginx/ssl/simonsmh.cc/fullchain.cer --key-file /etc/nginx/ssl/simonsmh.cc/privkey.key --reloadcmd "service nginx force-reload"
```

## nginx 需要的证书位置  
```bash
#certificate
/etc/letsencrypt/live/simonsmh.tk/fullchain.pem
#privatekey
/etc/letsencrypt/live/simonsmh.tk/privkey.pem
```

## nginx配置示例
```nginx
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
