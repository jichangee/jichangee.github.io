---
layout:     post   				    # 使用的布局（不需要改）
title:      Electron安装流程 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2018-08-24 				# 时间
author:     moxuy 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - electron
---

## 正常安装
在终端执行命令
```
npm i -g electron-prebuilt
```
可能的错误：
> connect ETIMEDOUT 54.231.49.112:443

如果碰到以上错误，就使用其他安装方式


##其他安装方式
到github上下载最新稳定版的electron，[下载连接](https://github.com/electron/electron/releases/tag/v1.4.16)。
我下载的版本是 [1.1.16](https://github.com/electron/electron/releases/download/v1.4.16/electron-v1.4.16-darwin-x64.zip)
再执行正常安装命令，待出现 node install.js 之后，按下control+c，中断安装。

进入npm全局安装路径，mac为 /usr/local/lib/node_modules。

在 electron-prebuilt 目录下创建 dist 文件夹和path.txt文本文件，

将下载的zip解压后的全部文件放入dist文件夹中

然后在path.txt文件中填入 dist/Electron.app/Contents/MacOS/Electron

目录结构如下
![electron.png](https://i.loli.net/2018/09/27/5bac72ef5b05b.png)
然后在终端中输入 electron -v 如果显示版本，那么代表安装成功。