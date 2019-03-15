---
date: 2018-12-2 20:02
tags: Django
status: public
title: 'Django 发布配置'
---
1、配置nginx
<code>sudo vim /etc/nginx/sites-available/AGAC.conf</code>
<code>sudo ln -s /etc/nginx/sites-available/AGAC.conf /etc/nginx/sites-enabled/AGAC.conf</code>
<code>$sudo /etc/init.d/nginx start</code>
```
server {
    listen      8000;
    server_name 主机ip;
    charset     utf-8;

    client_max_body_size 75M;
    access_log /home/admin/AGAC_web/AGAC/nginx/acc_log;
    error_log  /home/admin/AGAC_web/AGAC/nginx/error_log;
    location /static {
        alias /home/admin/AGAC_web/AGAC/static;
    }
    location / {
        uwsgi_pass  127.0.0.1:8899;
        include     /etc/nginx/uwsgi_params;
    }
}
```
1、配置uwsgi
在项目目录中新建uwsgi.ini
<code>sudo uwsgi --ini uwsgi.ini</code>
```
[uwsgi]
socket= 127.0.0.1:8899
chdir = /home/admin/AGAC_web/AGAC
wsgi-file = AGAC/wsgi.py
processes = 2
threads = 4
module=AGAC.wsgi
#chmod-socket = 664
#chown-socket = tu:www-data
vacuum = true
daemonize = UWSGI.log
stats=%(chdir)/uwsgi/uwsgi.status
pidfile=%(chdir)/uwsgi/uwsgi.pid
```

3、静态文件搜集
```
STATIC_ROOT = os.path.join(BASE_DIR, "static/")
$python manage.py collectstatic
```

sudo killall -9 uwsgi

参考：https://blog.csdn.net/mr_oldcold/article/details/79833044
https://segmentfault.com/q/1010000002523354












