---
title: Debian安装seafile+MySQL
date: 2016-05-07 17:02:00
tags: [Linux]
---

### 安装seafile
```
apt update
apt install python2.7 python-setuptools python-imaging python-ldap python-mysqldb python-memcache
wget http://download-cn.seafile.com/seafile-server_5.1.1_x86-64.tar.gz
tar -xzf seafile-server_*
cd seafile-server-*
./setup-seafile-mysql.sh #安装
./seafile.sh start && ./seahub.sh start
```
### 开机启动
```
nano /etc/init/seafile-server.conf #设置开机脚本

start on (started mysql
and runlevel [2345])
stop on (runlevel [016])

pre-start script
/etc/init.d/seafile-server start
end script

post-stop script
/etc/init.d/seafile-server stop
end script

####################

nano /etc/init.d/seafile-server && chmod a+x /etc/init.d/seafile-server && update-rc.d seafile-server defaults #写入开机脚本
#!/bin/sh
### BEGIN INIT INFO
# Provides:          seafile-server
# Required-Start:    $syslog $remote_fs $network $local_fs
# Required-Stop:     $syslog $remote_fs $network $local_fs
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: lightweight secured seafile
# Description: seafile server
### END INIT INFO
user=root
seafile_dir=/root
script_path=${seafile_dir}/seafile-server-latest
seafile_init_log=${seafile_dir}/logs/seafile.init.log
seahub_init_log=${seafile_dir}/logs/seahub.init.log
fastcgi=false
fastcgi_port=8000

case "$1" in
        start)
                sudo -u ${user} ${script_path}/seafile.sh start >> ${seafile_init_log}
                if [  $fastcgi = true ];
                then
                        sudo -u ${user} ${script_path}/seahub.sh start-fastcgi ${fastcgi_port} >> ${seahub_init_log}
                else
                        sudo -u ${user} ${script_path}/seahub.sh start >> ${seahub_init_log}
                fi
        ;;
        restart)
                sudo -u ${user} ${script_path}/seafile.sh restart >> ${seafile_init_log}
                if [  $fastcgi = true ];
                then
                        sudo -u ${user} ${script_path}/seahub.sh restart-fastcgi ${fastcgi_port} >> ${seahub_init_log}
                else
                        sudo -u ${user} ${script_path}/seahub.sh restart >> ${seahub_init_log}
                fi
        ;;
        stop)
                sudo -u ${user} ${script_path}/seafile.sh $1 >> ${seafile_init_log}
                sudo -u ${user} ${script_path}/seahub.sh $1 >> ${seahub_init_log}
        ;;
        *)
                echo "Usage: /etc/init.d/seafile-server {start|stop|restart}"
                exit 1
        ;;
esac
```
### 高级配置
```
rm -rf /root/conf/seahub_settings.pyc
nano /root/conf/seahub_settings.py #添加推荐高级配置
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.memcached.MemcachedCache',
        'LOCATION': '127.0.0.1:11211',
    }
}

USE_PDFJS = True
FILE_PREVIEW_MAX_SIZE = 30 * 1024 * 1024
ENABLE_THUMBNAIL = True
THUMBNAIL_ROOT = '/tmp/seahub-data/thumbnail/thumb/'
EMAIL_USE_TLS = True

EMAIL_HOST = 'smtp.google.com'
EMAIL_HOST_USER = 'simonsmh@gmail.com'
EMAIL_HOST_PASSWORD = '***************'
EMAIL_PORT = '587'
DEFAULT_FROM_EMAIL = EMAIL_HOST_USER
SERVER_EMAIL = EMAIL_HOST_USER
```