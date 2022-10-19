---
title: git使用
comments: true
copyright: true
tags:
  - git
categories:
  - git
abbrlink: 4896de77
date: 2022-08-29 20:02:39
---

1, 配置全局的用户名和邮箱

git config --global user.name "注册的用户名"

git config --global user.email "注册的邮箱"

2,生成SSH KEY命令:ssh-keygen -t rsa -C "注册的邮箱"

将.ssh下的id_rsa.pub内容复制到github中

用户中心--->settings--->左侧的SSH KEYS,权限RW,点击添加按钮

3, 文件上传

git init

git add .

git commit -m ""

git remote add origin 远程库地址

git push -u origin master / git push -f origin master



1,常用(上传代码)
git stash
git pull
git stash pop
git add .
git commit -m ""
git push
扩展:
git checkout 分支名:切换分支
git pull origin master: 拉取主分支代码
git merge dev: 合并dev分支代码到主分支

![git使用](https://img-blog.csdnimg.cn/eb1e2203dd17421ebbf075e02cda5e0c.png)

1, 配置git
git config –global user.name “注册的用户名”
git config –global user.email “注册的邮箱”
2, 生成ssh配置文件
①生成SSH KEY命令:ssh-keygen -t rsa -C “注册的邮箱”
②将.ssh下的id_rsa.pub内容复制到github中
③用户中心—>settings—>左侧的SSH KEYS,权限RW,点击添加按钮
3,文件上传
git init
git add .
git commit -m “”
git remote add origin 远程库地址
git push -u origin master / git push -f origin master
4,常用文件上传（上传到远程分支）
git stash
git pull
git stash pop
git add .
git commit -m “”
git push
5, 文件下载
git clone -b 分支名 版本库地址
6, git 上传代码到远程分支
git branch # 查看分支 
git checkout -b 分支名 #创建并切换分支
git add .  #添加文件到暂存区
git push origin 分支名 #提交分支
7,  分支合并
git checkout master #切换到主分支
git merge 分支名 #合并分支到主分支



