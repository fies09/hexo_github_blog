---
title: nginx部署
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

###### 方案 1：uwsgi+nginx部署

uwsgi的安装: pip install uwsgi

创建配置文件: uwsgi.ini

uwsgi.ini 配置文件内容

```python
[uwsgi]
# uWSGI 监听的 UNIX Socket
socket = uwsgi.sock

#项目目录
chdir=/Users/fanyong/Desktop/code/DjangoBlog

#项目中wsgi.py文件的目录，相对于项目目录
wsgi-file=djangoblog/wsgi.py

#进程数
processes=2

#线程数
threads=2

#是否开启master进程
master=True

#存放进程编号的文件
pidfile=uwsgi.pid

#日志文件，因为uwsgi可以脱离终端在后台运行，日志看不见。我们以前的runserver是依赖终端的
daemonize=uwsgi.log

#指定依赖的虚拟环境
virtualenv=/Users/fanyong/mambaforge/envs/DjangoBlog

# 设置 Socket 访问权限
chmod-socket = 666

# 确保 uWSGI 在项目代码更新后自动重启
touch-reload = djangoblog/wsgi.py
```

uwsgi模块常用命令：

uwsgi启动: uwsgi --ini uwsgi.ini

uwsgi重启: uwsgi --reload uwsgi.pid

uwsgi停止: uwsgi --stop uwsgi.pid



服务器上nginx安装配置：

更新系统：

```
sudo apt update && sudo apt upgrade -y
```

安装依赖：

```
apt install python3-pip python3-venv nginx -y
```

设置开机自启：

```
sudo systemctl start gunicorn
sudo systemctl enable gunicorn
```

检查Gunicorn状态：

```
sudo systemctl status gunicorn
```

配置nginx：

nginx配置文件路径介绍：

配置目录：/etc/nginx
执行文件: /usr/sbin/nginx
日志目录：/var/log/nginx
启动文件：/etc/init.d/nginx
web目录：/var/www/html/，首页文件是index.nginx-debian.html

1，查看服务状态:     systemctl status nginx / service nginx status

2，创建 nginx配置 文件

​	2.1, 生成私钥：

```
sudo mkdir -p /etc/nginx/ssl
cd /etc/nginx/ssl

# 生成私钥
sudo openssl genpkey -algorithm RSA -out nginx-selfsigned.key

# 生成证书请求（CSR）
sudo openssl req -new -key nginx-selfsigned.key -out nginx-selfsigned.csr \
    -subj "/C=CN/ST=Shanghai/L=Shanghai/O=MyCompany/OU=IT/CN=127.0.0.1"

# 生成自签名证书（有效期 365 天）
sudo openssl x509 -req -days 365 -in nginx-selfsigned.csr -signkey nginx-selfsigned.key -out nginx-selfsigned.crt
```

​	2.1.1, 更新nginx.conf

```
ssl_certificate /etc/nginx/ssl/nginx-selfsigned.crt;
ssl_certificate_key /etc/nginx/ssl/nginx-selfsigned.key;
```

​	

2.2, 在 nginx.conf 文件当中追加

```
# 追加内容
include /opt/homebrew/etc/nginx/conf.d/*.conf;
```

​	2.3, 在指定目录下创建对应的配置文件（eg:djangoblog.conf）

```
server {
    listen 443 ssl;
    server_name 127.0.0.1;

    ssl_certificate /etc/nginx/ssl/nginx-selfsigned.crt;
    ssl_certificate_key /etc/nginx/ssl/nginx-selfsigned.key;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    root /Users/fanyong/Desktop/code/DjangoBlog;

    location /static/ {
        alias /Users/fanyong/Desktop/code/DjangoBlog/collectedstatic/;
    }

    location /media/ {
        alias /Users/fanyong/Desktop/code/DjangoBlog/uploads/;
    }

    location / {
        include uwsgi_params;
        uwsgi_pass unix:/Users/fanyong/Desktop/code/DjangoBlog/uwsgi.sock;
    }
}

server {
    listen 80;
    server_name 127.0.0.1;
    return 301 https://$host$request_uri;
}

```

检查配置文件是否有问题:     nginx -t
重新加载配置文件:     nginx -s reload

访问项目！



###### 方案 2: gunicorn+nginx

创建**Gunicorn systemd 服务**并完成开机自启：vim /etc/systemd/system/gunicorn.service

```
# 替换your_user和myproject
[Unit]
Description=Gunicorn daemon for Django project
After=network.target

[Service]
User=your_user
Group=www-data
WorkingDirectory=/var/www/myproject
ExecStart=/var/www/myproject/venv/bin/gunicorn --workers 3 --bind unix:/var/www/myproject/gunicorn.sock myproject.wsgi:application

[Install]
WantedBy=multi-user.target
```

设置开机自启：

```
sudo systemctl start gunicorn
sudo systemctl enable gunicorn
```

检查Gunicorn状态：

```
sudo systemctl status gunicorn
```

静态资源文件的处理

 在settings.py中配置静态资源文件路径：

```
STATIC_ROOT = "/var/www/myproject/static/"
```

静态资源文件的生成：python manage.py collectstatic --noinput

gunicorn 启动：gunicorn --workers 3 --bind unix:/tmp/gunicorn.sock djangoblog.wsgi

以守护进程的方式启动：gunicorn --workers 3 --bind unix:/tmp/gunicorn.sock djangoblog.wsgi --daemon

nginx配置文件:
/etc/nginx/nginx.conf

```python
server {
    listen 443 ssl;
    server_name localhost;

    ssl_certificate /etc/nginx/ssl/nginx-selfsigned.crt;
    ssl_certificate_key /etc/nginx/ssl/nginx-selfsigned.key;

    location / {
        # dev
        proxy_pass http://unix:/tmp/gunicorn.sock;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location /static/ {
		alias /Users/fanyong/Desktop/code/DjangoBlog/collectedstatic/;
    }

    location /media/ {
        alias /Users/fanyong/Desktop/code/DjangoBlog/uploads/;
    }
}
```



检查配置文件是否有问题:     nginx -t
重新加载配置文件:     nginx -s reload

访问项目！

补充说明，以下和在nginx中配置为两个方法，以下开发中使用。

ssl: uwsgi免费获取SSL证书方法如下：

1，执行如下命令

```
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d your_domain
```

自动续订：

```
sudo certbot renew --dry-run
```



2，nginx 配置

```
server {
    listen 443 ssl;
    server_name 127.0.0.1;

    ssl_certificate /etc/letsencrypt/live/your_domain/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/your_domain/privkey.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;

    root /Users/fanyong/Desktop/code/DjangoBlog;

    location /static/ {
        alias /Users/fanyong/Desktop/code/DjangoBlog/collectedstatic/;
    }

    location /media/ {
        alias /Users/fanyong/Desktop/code/DjangoBlog/uploads/;
    }

    location / {
        include uwsgi_params;
        uwsgi_pass unix:/Users/fanyong/Desktop/code/DjangoBlog/uwsgi.sock;
    }
}

server {
    listen 80;
    server_name 127.0.0.1;

    # 自动重定向 HTTP 到 HTTPS
    return 301 https://$host$request_uri;
}
```

**说明：**

​	1.	**证书路径**：替换了自签名证书的路径（/etc/nginx/ssl/nginx-selfsigned.crt 和 /etc/nginx/ssl/nginx-selfsigned.key），改为 certbot 获取的证书路径：

​	•	/etc/letsencrypt/live/your_domain/fullchain.pem：证书链文件。

​	•	/etc/letsencrypt/live/your_domain/privkey.pem：私钥文件。

​	2.	**HTTP 到 HTTPS 重定向**：确保所有从 HTTP 端口（80）进入的请求都会被自动重定向到 HTTPS（443）端口。

​	3.	**续订检查**：使用 certbot renew --dry-run 命令来模拟续订过程，确保 SSL 证书的续订顺利进行。

​	4.	**注意**：将 your_domain 替换为实际的域名，certbot 会自动为你配置 SSL 证书。



当你完成配置后，运行 sudo certbot --nginx -d your_domain 来获取免费的 SSL 证书，并自动更新你的 Nginx 配置。



gunicorn 免费获取SSL证书方法如下：

nginx 配置：

```
server {
    listen 443 ssl;
    server_name localhost;

    ssl_certificate /etc/letsencrypt/live/your_domain/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/your_domain/privkey.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;

    location / {
        # dev
        proxy_pass http://unix:/tmp/gunicorn.sock;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location /static/ {
        alias /Users/fanyong/Desktop/code/DjangoBlog/collectedstatic/;
    }

    location /media/ {
        alias /Users/fanyong/Desktop/code/DjangoBlog/uploads/;
    }
}

server {
    listen 80;
    server_name localhost;

    # 自动重定向 HTTP 到 HTTPS
    return 301 https://$host$request_uri;
}
```

**说明：**

​	1.	**证书路径**：替换了自签名证书的路径，改为 certbot 获取的证书路径：

​	•	/etc/letsencrypt/live/your_domain/fullchain.pem：证书链文件。

​	•	/etc/letsencrypt/live/your_domain/privkey.pem：私钥文件。

​	•	请将 your_domain 替换为你实际的域名。

​	2.	**SSL 配置**：

​	•	启用 TLSv1.2 和 TLSv1.3 协议。

​	•	强制使用高强度的加密套件，禁用 NULL 和 MD5。

​	•	启用 ssl_prefer_server_ciphers，使服务器优先使用指定的加密套件。

​	3.	**HTTP 到 HTTPS 重定向**：确保所有从 HTTP 端口（80）进入的请求都被自动重定向到 HTTPS（443）端口。



**自动续订：**

确保定期续订 SSL 证书，可以设置自动续订（例如通过 cron 任务）。你可以运行以下命令来模拟续订：

```
sudo certbot renew --dry-run
```

**使用 certbot 获取证书：**

如果尚未为 your_domain 获取证书，可以使用以下命令：

```
sudo certbot --nginx -d your_domain
```

确保 gunicorn 和 Nginx 都配置正确，这样可以顺利通过 HTTPS 提供服务。
