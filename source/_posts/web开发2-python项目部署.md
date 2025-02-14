---
title: web开发2_python项目部署
comments: true
copyright: true
tags:
  - web开发
  - python
categories:
  - web开发
abbrlink: dbe3cbd9
date: 2022-08-29 21:00:01
---

pm2运行命令:

```
pm2 ls
pm2 start xxx.py --name mynodeapp
pm2 startup
pm2 save
pm2 stop
pm2 log
```

python文件运行命令:

```
nohup python3 -u risk_sql.py >risk.log &
pm2 start /home/project/net_diagnose/sanit.py --interpreter python3 --name sanit_script
```

运行flask:

```
env FLASK_APP=scheduler.py flask run -h 0.0.0.0 -p 5008
```

通过 gunicorn 启动

```
nohup gunicorn -b 0.0.0.0:9000 -w 4 app:app > nohup.log &
说明：
	-b： bind，ip+port
	# mac:echo $(( $(sysctl -n hw.ncpu) * 2 + 1 ))	linux: echo $(( $(nproc) * 2 + 1 ))
	-w:	 workers:cpu 核心数 x 2 + 1s 	
	app:app	模块名:应用对象名
			•	第一个 app：Python 文件 app.py（即 app.py 文件）。
			•	第二个 app：在 app.py 文件中定义的 Flask 应用对象（例如 app = Flask(__name__)）。
			适用于 Flask。如果是 Django，通常是 myproject.wsgi。
```

Supervisor：进程管理工具

安装：

```
linux:
sudo apt update && sudo apt install supervisor -y / sudo yum install supervisor -y
mac：
brew install supervisor
```

配置：位于 /etc/supervisor/conf.d/ 目录，每个进程都有一个单独的 .conf 文件。

```
[program:gunicorn]
command=/usr/local/bin/gunicorn -w 4 -b 0.0.0.0:8000 app:app
directory=/home/user/myproject
autostart=true
autorestart=true
stderr_logfile=/var/log/gunicorn.err.log
stdout_logfile=/var/log/gunicorn.out.log
```

启动并加载配置：

```
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start gunicorn
# 查看进程状态
sudo supervisorctl status
# 停止进程
sudo supervisorctl stop gunicorn
# 重启进程
sudo supervisorctl restart gunicorn
# 重新加载所有进程
sudo supervisorctl reload
```

supervisorctl + **Django / Flask + Gunicorn** / supervisorctl + celery

eg1：**Django + Gunicorn + Supervisor + Nginx**

创建supervisor配置： vim /etc/supervisor/conf.d/django_gunicorn.conf

```
# gunicorn
#[program:django_gunicorn]
#command=/usr/local/bin/gunicorn -w 4 -b 127.0.0.1:8000 myproject.wsgi:application
# uwsgi
# [program:uwsgi]
#command=/usr/local/bin/uwsgi --ini /home/user/myproject/uwsgi.ini
# celery
[program:celery]
command=/usr/local/bin/celery -A myproject worker --loglevel=info
directory=/home/user/myproject
autostart=true
autorestart=true
stderr_logfile=/var/log/gunicorn.err.log
stdout_logfile=/var/log/gunicorn.out.log
```

启动：

```
sudo supervisorctl reread
sudo supervisorctl update
# sudo supervisorctl start django_gunicorn
# sudo supervisorctl start uwsgi
sudo supervisorctl start celery
```

配置 nginx，参考 nginx 部署

启动 nginx 并重启：

```
sudo ln -s /etc/nginx/sites-available/myproject /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```



**Docker + Gunicorn + Nginx + PostgreSQL/MySQL/Redis**容器化部署

安装：

```
# 安装 Docker（Ubuntu）
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable docker
sudo systemctl start docker

# 安装 Docker Compose
sudo apt install -y docker-compose
```

验证安装

```
docker --version
docker-compose --version
```

目录结构：

```
myproject/
│── app/                  # Python Web 应用代码
│   ├── main.py           # Flask/FastAPI 入口文件 或 Django manage.py
│   ├── requirements.txt  # Python 依赖包
│   ├── Dockerfile        # Docker 镜像构建文件
│── nginx/                # Nginx 配置
│   ├── default.conf
│── docker-compose.yml    # Docker Compose 配置
│── .env                  # 环境变量文件
```

flask包（flask	gunicorn）

django包（django	gunicorn	psycopg2  # PostgreSQL 数据库驱动）

编写Dockerfile

```
# 选择基础镜像
FROM python:3.10

# 设置工作目录
WORKDIR /app

# 复制项目文件
COPY . /app/

# 安装依赖
RUN pip install --no-cache-dir -r requirements.txt

# 运行 Gunicorn 服务器
CMD ["gunicorn", "-b", "0.0.0.0:5000", "main:app"]
```

django如下：

```
CMD ["gunicorn", "-b", "0.0.0.0:8000", "myproject.wsgi:application"]
```

配置nginx(参考nginx部署):

编写**docker-compose.yml**

```
version: "3.8"

services:
  app:
    build: .
    container_name: python_app
    restart: always
    ports:
      - "5000:5000"
    environment:
      - DEBUG=False
    depends_on:
      - db

  db:
    image: postgres:13
    container_name: postgres_db
    restart: always
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydatabase
    volumes:
      - postgres_data:/var/lib/postgresql/data

  nginx:
    image: nginx:latest
    container_name: nginx_server
    restart: always
    ports:
      - "80:80"
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - app

volumes:
  postgres_data:
```

docker镜像构建：

```
docker-compose build
```

启动所有容器：

```
docker-compose up -d
```

查看运行状态：

```
docker ps
```

进入容器：

```
docker exec -it python_app bash
```

停止和删除容器

```
docker-compose down
```

 **配置 docker-compose.override.yml（可选）**

用于本地开发（映射本地代码，实时更新）：

```
version: "3.8"

services:
  app:
    volumes:
      - .:/app
```

自动重启 Docker容器

```
sudo systemctl enable docker
```

使用Supervisor 管理

```
[program:docker_compose]
command=/usr/local/bin/docker-compose up
directory=/home/user/myproject
autostart=true
autorestart=true
stderr_logfile=/var/log/docker.err.log
stdout_logfile=/var/log/docker.out.log
```

启动：

```
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start docker_compose
```

docker部署 HTTPS(SSL 证书)

方法 1：**使用 Let’s Encrypt + Certbot**

使用 ssl 证书

```
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d mydomain.com
```

自动续订

```
sudo certbot renew --dry-run
```

方法 2：nginx 配置：

```
server {
    listen 443 ssl;
    server_name mydomain.com;

    ssl_certificate /etc/nginx/ssl/fullchain.pem;
    ssl_certificate_key /etc/nginx/ssl/privkey.pem;

    location / {
        proxy_pass http://app:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

