---
title: scrapy框架使用
comments: true
copyright: true
tags:
  - 爬虫
  - scrapy
categories:
  - 爬虫
abbrlink: 1d5ab057
date: 2022-08-29 20:50:08
---

1,Scrapy的安装配置(根据个人实际情况,每个人项目不一样)
①,先安装将twisted文件放到E盘根目录下
#前提必须安装wheel模块,为了安装whl文件
②,E:—>pip install Twisted-18.4.0-cp36-cp36m-win_amd64.whl
③,pip install scrapy==1.5.0

2,在E盘创建Scrapy工程:
E: —> scrapy startproject doubanmovie —>
cd doubanmovie —>scrapy genspider moviespider douban.com

3,爬虫主程序的编写:
在Pycharm工具打开doubanmovie下级目录的doubanmovie
①,添加浏览器标识
将rotate_useragent文件放到doubanmovie文件夹下
②,在settings中的DOWNLOAD_MIDDLEWARES中添加配置信息
添加禁用框架自带的浏览器标识及设置浏览器标识
#禁用框架自带的浏览器标识
‘scrapy.contrib.downloadermiddleware.useragent.UserAgentMiddleware’: None,
#设置浏览器标识
‘doubanmovie.rotate_useragent.RotateUserAgentMiddleware’:400
②-①,编写settings,py,启动管道组件,ITEM_PIPELINES 以及其他相关设置

③,编写主程序

④,执行
scrapy crawl (spiders下文件,不包含后缀)

注意:设置浏览器标识时,第一个应为项目名

爬虫爬取数据流程(scrapy)
1,scrapy startproject XXXX
2,scrapy genspider XXXX “http://www.XXXX.com”
3,编写item.py,明确需要提取的数据
4,编写spiders/xxxx.py ,编写爬虫文件,处理请求和响应,以及提取数据(yield item )
5,编写pipelines.py , 编写管道文件,处理spider返回item,比如本地持久化存储等…
6,编写settings,py,启动管道组件,ITEM_PIPELINES 以及其他相关设置
7,执行爬虫
