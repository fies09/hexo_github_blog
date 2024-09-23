---
title: docker面试题.md
comments: true
copyright: true
tags:
  - docker
  - 面试题
categories:
  - 面试题
abbrlink: aeec021b
date: 2024-09-23 23:55:03
---

1, dockerfile 文件当中 copy 和 add 的区别是什么？

copy: 只进行复制文件和目录，不会解压归档文件或者从 url 进行下载。

add:  会自动解压归档文件，可以从指定 url 下载文件并添加到镜像中。

