---
title: 项目整理.md
comments: true
copyright: true
tags:
  - 项目内容整理
  - 面试题
categories:
  - 面试题
abbrlink: 6f70a492
date: 2024-09-24 09:27:58
---

1，文本转语音采用 ffmeg 工具进行转换。websocket

​     通过pydub包模块实现文本转语音. 		http 请求

2，语音转文本：采用vosk模型进行语音识别，将识别结果通过 websocket 的方式返回给前端									websocket

​							 采用vosk模型进行语音识别，将识别结果通过http 请求的方式将结果返回给前端                               http 请求																																																

3, 知识图谱，读取不同 excel 表格的文件内容，然后建立对应的节点与关系。建立完成之后通过检查数据判断是否正常写入。

