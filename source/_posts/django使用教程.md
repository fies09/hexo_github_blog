---
title: django使用教程
comments: true
copyright: true
tags:
  - web
  - django
categories:
  - web
abbrlink: ba447774
date: 2022-08-29 19:56:40
---

1、创建Django项目(在指定目录下)
命令：django-admin startproject 项目名

2、创建Django应用
一个项目由很多个应用组成的，每一个应用完成一个功能模块。
创建应用的命令如下：(进入项目文件夹中)
python manage.py startapp 应用名


3, 在django项目中配置数据库连接信息
'default': {
        'ENGINE': 'django.db.backends.mysql',
	#数据库名（需要提前创建）
        'NAME': 'db_django',
        #用户名
        'USER':'root',

		#密码
	    'PASSWORD':'root',
	    #url 服务器地址
	    'HOST':'localhost',
	    #端口号
	    'PORT':3306,
	}


4,更新应用数据库
python manage.py makemigrations
python manage.py migrate

5,创建超级管理员
python manage.py createsuperuser

6,运行项目
python manage.py runserver

7,admin管理员页面汉化:
setting中修改为:
LANGUAGE_CODE = 'zh-Hans'
TIME_ZONE = 'Asia/Shanghai'
