---
title: svn使用
comments: true
copyright: true
tags:
  - svn
categories:
  - svn
abbrlink: df8f2d0d
date: 2022-08-29 20:51:16
---

[软件地址](https://pan.baidu.com/s/1WGIjMTBU_7J9BLoiBHbSjw)

提取码: 4iuo

[汉化包地址](https://pan.baidu.com/s/1eWpX4ai5_eTaLHCn0x1_hQ)

提取码: 7u70 

汉化说明:

在任意位置右击--->TortoiseSVN--->settings--->General--->Language(简体中文)--->应用 ---> 确定

1, 导出项目

在检出目录文件夹下:

右击--->SVN检出--->填写版本库url--->确定

**版本库url为服务器地址,检出标志为绿色的√



2, 修改文件,添加文件,删除文件

	2.1 添加文件
	
		选中 ---> 右击 ---> 加入 (会出现蓝色感叹号) --- > 提交

总结: add ,commit

	2.2 修改文件
	
		检出要修改的文件 ---> 右击 ---> SVN更新 (对文件内容进行修改,保存,'会出现红色感叹号') ---> 提交(对提交文件进行相关备注)

格式:

	*XXXX年—XX月—XX日 姓名 修改内容：XXXXXXXX 修改原因：YYYYYYYYYYYYY\*

	2.3 重命名文件
	
		选中文件--->SVN更新--->修改文件名--->add--->commit
	
	2.4删除文件(删除前备份)
	
		选中文件---> SVN更新 ---> 删除 ---> 提交

总结:

	更新,提交,显示日志,改名,删除

checkout:将文件添加到本地目录

add: 往版本库添加新的文件

commit: 将改动的文件提交到版本库

lock/unlock: 加锁/解锁

update: 更新

delete: 删除
