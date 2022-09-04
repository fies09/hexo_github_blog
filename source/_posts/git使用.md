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





