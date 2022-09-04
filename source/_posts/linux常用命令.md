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
