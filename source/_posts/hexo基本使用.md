---
title: hexo基本使用
comments: true
copyright: true
tags:
  - blog
  - hexo
categories:
  - hexo
abbrlink: d4fee3ae
date: 2022-08-29 20:06:33
---

1, [下载nodejs](https://nodejs.org/en/download/)

2,  下载淘宝镜像

npm install -g cnpm --registry=https://registry.npm.taobao.org

3, 下载hexo

cnpm install -g hexo-cli

4, 建立文件夹,生成博客

mkdir blog

cd blog 

hexo init

下载next主题: 

git clone https://github.com/litten/hexo-theme-next.git themes/next

在github上建立仓库: 

github名.github.io

安装git相关模块
npm install --save hexo-deployer-git

hexo clean && hexo g && hexo d/hexo clean && hexo g && hexo s



