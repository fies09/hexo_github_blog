---
title: linux面试题
comments: true
copyright: true
tags:
  - linux面试题
  - 面试题
categories:
  - 面试题
abbrlink: e171c965
date: 2022-11-25 15:40:52
---

1, [常用命令](https://blog.csdn.net/Dabie_haze/article/details/118969328?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166920428516800215048409%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166920428516800215048409&biz_id=0&utm_medium=distribute.pc_chrome_plugin_search_result.none-task-blog-2~all~top_click~default-4-118969328-null-null.nonecase&utm_term=linux%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4&spm=1018.2226.3001.4187)

1. ls/ll：列出文件list

2. cd：切换目录change directory

3. cp：复制copy

4. mv：移动move

5. rm：移除，删除remove

6. mkdir：创建文件夹make directory

7. rmdir：移除，删除文件夹remove directory

8. chown：更改所有者change owner

9. chmod：更改文件的权限模式change mode

10. find：查找

11. |：管道

12. grep：按行查找并匹配

13. tar：打包，压缩，解压

14. cat：打印文件内容

15. ps：查看进程process select

16. kill：杀死进程

17. passwd：修改密码password

18. pwd：显示工作目录print work directory

19. tee：显示并保存

20. reboot：重启

21. lsof/netstat: 查看端口 是否被占用
    lsof -i:22
    netstat -tunlp|grep 22
    
22. tail: 查看日志
    实时查看:
    tail -f 日志名
    查看后200行日志内容
    tail -f -n 200 demo.log  
    
22. top: 查看cpu占用率
    
22. netstat命令 – 显示网络状态
    
    

2, uwsgi和nginx
[详解](https://fies09.github.io/article/3c1ae316.html#more)
uwsgi: web服务器(应用服务器),用于连接Web服务器和Web应用框架
uwsgi启动:
uwsgi –ini uwsgi8001.ini
uwsgi重启：
uwsgi –reload uwsgi8001.pid
uwsgi停止:
uwsgi –stop uwsgi8001.pid
nginx: 是一个高性能、轻量级的http和反向代理服务器



3, pm2 . nohup
pm2: 进程管理工具
pm2 ls
pm2 start
pm2 stop
pm2 log
pm2 start Tasker/schedule_task.py -x --interpreter python3  -o ./logs/node_task.log -e ./logs/node_task_error.log
nohup:  后台运行项目
保存日志:
nohup python3 -u Main.py > nohup.log &
不保存日志:
nohup python3.6 /opt/moss_robot/lib/dispatch_v5.3.2/robot_wait.py >/dev/null 2>&1 &



4, docker:
安装:apt-get/yum install docekr
查看服务状态:systemctl status docker
启动服务:systemctl start docker
拉取镜像:docker pull centos/ubuntu
进入镜像:docker run -it ubuntu /bin/bash
退出:exit
查看所有容器:docker ps -a
启动容器:docker start id
进入容器:docker exec -it id bash
重启:docker restart id



5,linux下如何设置,查看和注销环境变量

"" 设置环境变量
export LD_LIBRARY_PATH=/the/path/you/want/set
"" 查看设置
echo $LD_LIBRARY_PATH
"" 清除环境变量
unset LD_LIBRARY_PATH



6, 查看网卡使用的网络带宽情况

yum/apt install libpcap nethogs -y

#使用方法

nethogs

​	# 网卡名称 DEV

查看指定网卡占用带宽的进程

nethogs 网卡名称

