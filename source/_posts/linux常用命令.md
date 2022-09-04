---
title: linux常用命令
comments: true
copyright: true
tags:
  - linux
categories:
  - linux
abbrlink: fe4ef317
date: 2022-08-29 20:11:51
---

1, 安装deb

dpkg -i 文件名

2, window与liunx的shell脚本编码问题
修改编码:
vim 文件名
esc :
set ff=unix
esc : wq

查看编码
esc :
set ff

3, 解决vmware tools失效
sudo apt-get autoremove open-vm-tools	//卸载已有的工具
sudo apt-get install open-vm-tools		//安装工具open-vm-tools
sudo apt-get install open-vm-tools-desktop  //安装open-vm-tools-desktop
重启: init 0/reboot

4, 查看进程：ps -ef | grep 文件名
杀死进程：kill -9 id

5, shell文件运行命令
chmod +x ./auto_download.sh && ./auto_download.sh

6,定时器
cat /etc/crontab



Ubuntu:
1, 安装防火墙
sudo su
apt-get install firewalld
查看状态
systemctl status firewalld
2,乌班图源的处理:
[pip源和软件源处理](https://blog.csdn.net/wssywh/article/details/79216437)
3,安装deb文件
dpkg -i xxx.deb
3,定时任务
待测: cat /etc/corntab
①先创建py文件
②在py文件下输入crontab -e
③设置执行时间间隔
![文件内容](https://img-blog.csdnimg.cn/d8720c2d022a485985c260796741466e.jpeg)
![常见的时间设置](https://img-blog.csdnimg.cn/f874919abf8145c88291ce275a54b608.jpeg)
4,端口/进程查看
查看端口是否占用
lsof -i:22
netstat -tunlp|grep 22
查看进程
ps aux|grep API.py
杀死进程
kill -9 
5,实时查看日志
tail -f demo.log

查看后200行日志内容

tail -f -n 200 demo.log 
6,服务器启动
nohup python3 /opt/moss_robot/lib/dispatch_v5.3.2/robot_wait.py >/dev/null 2>&1 &
7,shell脚本运行
chmod +x ./start.sh && ./start.sh
