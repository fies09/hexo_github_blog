---
title: web开发1_nginx部署
comments: true
copyright: true
tags:
  - web开发
  - nginx
categories:
  - web开发
abbrlink: 3c1ae316
date: 2022-08-29 20:21:10
---

uwsgi的安装:
pip install uwsgi

创建配置文件:
uwsgi8001.ini

uwsgi启动:
uwsgi --ini uwsgi8001.ini

uwsgi重启：
uwsgi --reload uwsgi8001.pid

uwsgi停止:
uwsgi --stop uwsgi8001.pid


uwsgi.ini 配置文件内容

[uwsgi]
#直接做web服务器使用，Django程序所在服务器地址
http=127.0.0.1:8001
#项目目录
chdir=/home/parallels/Desktop/meiduo_mall
#项目中wsgi.py文件的目录，相对于项目目录
wsgi-file=meiduo_mall/wsgi.py
进程数

processes=2
线程数

threads=2
是否开启master进程

master=True
存放进程编号的文件

pidfile=uwsgi.pid
日志文件，因为uwsgi可以脱离终端在后台运行，日志看不见。我们以前的runserver是依赖终端的

daemonize=uwsgi.log
指定依赖的虚拟环境

virtualenv=/home/parallels/.virtualenv/django_py3

我们使用apt-get安装:     apt-get install nginx -y
查看服务状态:     systemctl status nginx / service nginx status
检查配置文件:     nginx -t
重新加载配置文件:     nginx -s reload

nginx目录介绍:
配置目录：/etc/nginx
执行文件: /usr/sbin/nginx
日志目录：/var/log/nginx
启动文件：/etc/init.d/nginx
web目录：/var/www/html/，首页文件是index.nginx-debian.html

nginx配置文件:
/etc/nginx/nginx.conf
