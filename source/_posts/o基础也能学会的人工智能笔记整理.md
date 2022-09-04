---
title: o基础也能学会的人工智能笔记整理
comments: true
copyright: true
tags:
  - AI
categories:
  - 数据处理
abbrlink: 2a662cad
date: 2022-08-29 20:25:50
---

1,机器学习
用大量数据进行训练，获取到 一个数据模型，预测就是应用训练的模型，来解决一个未知的问题

2,knn实现流程
收集相关的数据
选择合适的feature和label
如果不知道如何选择feature可以先单独每个feature计算与label的相关度
选取合适的k
使用数据集进行新的预测

3,分类和回归的区别(classification和regression)
回归是求topk的value求平均值
分类是求topk中出现最多的 类别

4,标准化和归一化的选择
知道数据范围用归一化
不知道数据范围,数据变化可能很大,用标准化

5,数据归一化
压缩样本数据到0~1之间,让向量之间的欧式距离变为标准欧式距离

6,线性回归和knn的区别
knn必须需要有全套数据集，每次预测都要重新计算整套数据集 
线性回归，数据集用完后，其实可以丢弃。 线性回归是算一个
模型。
knn可以理解成是数学统计学的方法研究问题 
线性回归，是一种总结规律，总结模型的解决问题的方法。

7,线性回归就是求线性函数的参数的值的过程

8,梯度下降算法原理
随机选取m和b
分别对mse 求m和b的偏导
如果m和b的偏导都很小(接近0),就成功
根据学习速率计算出修改的值
b和m 分别减去要修改的值

9,线性回归和逻辑回归的区别
线性回归:预测的数据是连续的
逻辑回归:预测的是分类的问题

10,人脸识别操作
安装百度api
pip install -U baidu-api


11,knn算法
调参:k
k一般取值总个数开平方
训练集的数据训练k.训练出的结果用测试集测试准确率

机器学习开发流程
1)获取数据
2)数据处理
3)特征工程
4)机器学习算法训练 - 模型
5)模型评估
6)应用
    

1,feature:变量  ---- label:结果
变量和结果一一对应
2,np.array():表示数组
3,abs:绝对值
4,predicePoint:预测点--->针对knn:k近邻
5,np.argsort():返回元素排序的下标位置
np.argsort(针对索引排序)
label ---> sortlabel
6,
np.sum:+  
np.squart:平方
np.sqrt:开平方跟
np.loadtxt:读取文件:    
属性
delimiter = "," :分隔符","
skiprows = 1    :空除第一行
usecols = (17,13,2):使用第17,13,2列
np.mean:平均值
np.stu:标准差
#显示完整的所有数据
np.set_printoptions(threshold=np.inf)
