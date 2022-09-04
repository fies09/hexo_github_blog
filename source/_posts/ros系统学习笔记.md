---
title: ros系统学习笔记
comments: true
copyright: true
tags:
  - linix
  - ros
Categories:
  - ros
abbrlink: 3e9342ec
date: 2022-08-29 20:48:52
---

[视频链接](https://www.bilibili.com/video/BV1qV41167d2?p=1)

环境安装说明:

 安装链接:

 	 [ROS安装](https://www.guyuehome.com/33971)

类似错误修复链接:

	[大部分错误针对修改有效](https://blog.csdn.net/qq_44830040/article/details/106049992)

安装步骤:

1,配置Ubuntu系统
打开软件中心,允许以下三种软件源
①restricted（不完全的自由软件）
②universe（Ubuntu官方不提供支持与补丁，全靠社区支持）
③multiverse（非自由软件，完全不提供支持和补丁）这三种软件源
下载地址:Download from为阿里云
http://mirrors.aliyun.com/ubuntu

2,打开终端,添加软件源
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

	2.1
	
			使用国内的镜像源,提高下载速度:(以下任意一个)
	
	             sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
	
	             sudo sh -c '. /etc/lsb-release && echo "deb http://mirror.sysu.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
	
	            sudo sh -c '. /etc/lsb-release && echo "deb http://ros.exbot.net/rospackage/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'



3,添加密钥(以下任意一个)
sudo apt-key adv ——keyserver hkp://ha.pool.sks-keyservers.net:80 ——recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116

sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

4,更新添加的软件源
sudo apt-get update

5,下载ros版本
sudo apt-get install ros-kinetic-desktop-full

6,初始化
sudo rosdep init

7,更新(实在不行此命令执行5~6次后可以忽略,暂时无影响)
rosdep update

8,设置ros环境变量,路径为home下ctrl + h打开隐藏文件就可看到

echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc

source ~/.bashrc

9,运行:roscore

三,测试是否安装成功
[链接地址](https://blog.csdn.net/dd_Mr/article/details/114323752)

①在终端运行以下命令开启ros
roscore
②打开一个新终端运行以下命令打开小乌龟窗口
rosrun turtlesim turtlesim_node
③打开新终端运行以下命令通过键盘控制小乌龟运动
rosrun turtlesim turtle_teleop_key
④打开新终端运行以下命令,看到ros的图形化界面,展示节点的关系,若终端无报错说明安装成功
rosrun rqt_graph rqt_graph

四,Ubuntu16.04上安装kitti2bag
[链接地址](https://blog.csdn.net/weixin_38636815/article/details/108376178)

①安装依赖包
sudo pip install pandas==0.23.0 -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com

sudo pip install pykitti -i http://pypi.douban.com/simple --trusted-host pypi.douban.com

sudo pip install kitti2bag  -i http://pypi.douban.com/simple --trusted-host pypi.douban.com

②安装kitti2bag包
sudo pip install kitti2bag

五,kitti2bag包对kitti数据集的处理
[数据集链接:](https://pan.baidu.com/s/1dpGDeYXYHHKtjmfX1IsVZw)
密码:oqym

打开下载的数据集,在2011_09_26目录的上一层目录下执行指令：
kitti2bag -t 2011_09_26 -r 0005 raw_synced .

生成一个ros可执行包:kitti_2011_09_26_drive_0005_synced.bag

执行以下命令打开图像可视化软件:
rqt_bag kitti_2011_09_26_drive_0005_synced.bag

读取图片:在文件上右键 ---> View ---> Image:
点击播放按钮一帧一帧的读取图片,一共15秒

缺点:只能读取图片,不能进行点云的读取

六:通过程序对kitti数据集进行处理
1,建立目录catkin_ws/src
2,在src目录下建包
catkin_create_pkg 包名(kitti_tutorial) rospy
3,进入包kitti_tutorial下建立资料夹:
catkin_make
4,在包下的src文件中编写程序,环境为python2,程序中无法写入中文,会报错,程序采用的是ASCII编码形式
5,程序完成后进入catkin_ws/src目录下执行以下命令:
roscd 包名(kitti_tutorial)/src

chmod +x 主程序(kitti.py)

rosrun 包名(kitti_tutorial) 主程序(kitti.py)

在新终端运行ros:
roscore

在新终端打开rviz:
rviz

在rviz中运行:
add ---> By topic(选择要运行的,例如图片,云点图,imu等)

保存:选择file--->save config as ---> 文件名 
