---
title: yolo学习笔记
comments: true
copyright: true
tags:
  - 目标识别
  - yolo
categories:
  - 目标识别
abbrlink: 4ada0a
date: 2022-08-29 21:01:34
---

1,目录详解

assets:需要测试的不同类型的文件

datasets:存放的数据集,例如Coco

demo:官方提供的4种部署方式

docs数据集信息

exps:Yolo所有版本的数据集

tools:程序入口

demo.py :  测试文件

train.py: 也是测试文件

weights:权重文件

yolox:模型代码

2,环境

pyhon环境/anaconda

pytorch

pycharm

下载环境:在根目录下输入pip install requirements.txt

3,运行

tools下的demo.py

parser.add_argument

约24行修改测试的类型 default = '类型'

约28行修改测试的内容 default = '源文件'

约39行修改权重的默认位置 default = '具体权重文件位置'

约43行修改默认的权重 default = '具体权重文件位置'

约44行修改默认的cpu/gpu default = '运行类型'

测试输出图像保存在tools/YOLOX_output

数据集测试:步骤

1,安装apex:

pip install apex

减少模型显存占用的工具

2,修改train文件中的参数

2-1,修改patch,默认50,约30行

2-2,数据集路径:在yolo_voc_nano.py下约54行的data_dir = '具体路径'

#55行修改:目录根据实际情况(可不修改)

train修改内容: 

具体的文件目录:

40行.

权重修改:

47行

GPU使用修改为True:

68行左右

输出内容主要有:

权重信息目录

训练完成后,利用权重进行测试

测试在demo目录下修改

类别和内容

权重文件和训练时的权重文件一致 .py         39行 : yolox_voc_nano.py

43 行权重改为训练后保存的权重文件             
