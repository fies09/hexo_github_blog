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
pm2 ls
pm2 start 
pm2 stop
pm2 log

python文件运行命令:
nohup python3 -u risk_sql.py >risk.log &

pm2 start /home/project/net_diagnose/sanit.py  -x --interpreter python3

python3 manage.py runserver 0.0.0.0:8000

鉴权:
基于BaseResource:测试注释掉,线上取消注释

运行flask:
env FLASK_APP=scheduler.py flask run -h 0.0.0.0 -p 5008

docker容器部署:
docker+Nginx+supervisor+gunicron

1. `docker exec -it saint_container bash`
2. `supervisorctl start monitor_business`

部署方式1:
	cd /home/project
	ll
	docker images
	docker ps -a
	cd net_diagnose
	cd /etc/supervisord.d/
	vi velery.ini
	cd /home/log
	vi saintwork.log
	cd ../project/net_diagnose
	supervisorctl
    
    
python3 -m pip install --upgrade pip
pip3 install -r requirements.txt -i  http://pypi.douban.com/simple --trusted-host pypi.douban.com

部署方式2:
nginx + pm2/uwsgi/gunicorn/supervisor + nohup
工具安装:
sudo apt-get install nginx

pip安装部署工具

pip install gunicorn
pip install supervisor
pip install uwsgi

安装PM2

sudo apt-get install -y nodejs / sudo yum -y install nodejs 
sudo npm install pm2 -g 

- **gunicorn部署**

gunicorn  -c 配置文件 运行模块名:应用名

  nohup gunicorn -b 0.0.0.0:9000 -w 4 --access-logfile=logs/access.log --error-logfile=logs/error.log app:app > nohup.log &

  nohup gunicorn -b 0.0.0.0:9000 -w 4 app:app > nohup.log &

开启异步并发

  nohup gunicorn -b 0.0.0.0:9000 -w 4 -k gevent app:app > nohup.log &

  其中 -k, --worker-class 可以指定异步进程类 需要先安装: `pip3 install psutil==5.7.0`

  pm2部署:

pm2 start app.py -x --interpreter python3  -o ./logs/access.log -e ./logs/error.log -i max

- **docker部署**

  docker start saint_container
  docker exec -it saint_container bash
  service supervisor start
  ps ef | grep nginx | grep -v grep | awk 'print {$2}' | xargs kill -9
  supervisord -c /etc/supervisor/supervisord.conf 
  supervisorctl

日志

  less -fN /opt/logs/process/gunicorn_out_port_9001.log
  less -fN /opt/logs/process/nginx_stdout.log
  less -fN /opt/logs/gunicorn/access.log
  less -fN /opt/logs/nginx/access.log


依赖安装:
pipreqs ./ --encoding=utf8 --force
pip3 install -r requirements.txt -i  http://pypi.douban.com/simple --trusted-host 

=== 源 ===

https://pypi.tuna.tsinghua.edu.cn/simple      # 清华源
https://mirrors.aliyun.com/pypi/simple/       # 阿里源
http://mirrors.cloud.tencent.com/pypi/simple  # 腾讯源
http://pypi.douban.com/simple/                # 豆瓣源pypi.douban.com

部署:

nohup:

nohup python3 -u server.py > nohup.log &

pm2:

pm2 start server.py -x --interpreter python3  -o ./logs/access.log -e ./logs/error.log

or

pm2 start ecosystem.config.js --env production

docker:

docker rmi $(docker images | grep "<none>"| awk '{print $3}')
./gen_image_push.sh
docker run -itd --name deploy -p 8000:8000 <container_id> 

tar xzvf aliyun-cli-linux-3.0.16-amd64.tgz

chmod +x ./scheduler.sh && ./scheduler.sh

部署:

1. 

项目部署:

方法一：

  nohup python3 -u Main.py > nohup.log &

方法二：

  pm2 start Main.py -x --interpreter python3  -o ./logs/pm2_info.log -e ./logs/pm2_error.log

方法三：

  supervisord -c /etc/supervisor.conf

or 

  cd Bin && sh start.sh

    # 任务部署:
    
    1. celery+supervisor
    
    supervisord -c /etc/supervisord.conf
    supervisorctl reload
    
    2. 定时框架+pm2
    
    pm2 start Tasker/schedule_task.py -x --interpreter python3  -o ./logs/node_task.log -e ./logs/node_task_error.log
    
    3. tornado+定时框架
    
    与tornado主程序一起执行
    
    查看rds是否在白名单

telnet rm-bp1ujkssv4gxkqqvv4o.mysql.rds.aliyuncs.com 3306
