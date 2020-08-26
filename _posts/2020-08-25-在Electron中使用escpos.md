---
layout:     post   				    # 使用的布局（不需要改）
title:      在Electron中使用escpos		  # 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2020-08-25 				# 时间
author:     薛纪昌 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Electron
    - NodeJs
---

## 前言
最近有个需求是需要在收银系统内连接热敏打印机和一体机的客显，因为项目中使用了electron进行开发并发布到Windows，所以就在网上找了下解决方案。
经过一番搜索，发现了一个[node-escpos](https://github.com/song940/node-escpos)库，但是相关开发资料比较少就只能自己摸索了。

## 热敏打印机
这块比较好对接，安装好相关依赖（```escpos```和```escpos-usb```）后，需要使用[electron-rebuild重建](https://github.com/electron/electron-rebuild#how-does-it-work)，因为其中使用到了一个原生模块（```usb```）。

安装完成之后只需按照[例子](https://github.com/song940/node-escpos#example)中操作即可打印出小票
