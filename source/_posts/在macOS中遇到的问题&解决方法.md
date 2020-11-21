---
layout:     post   				    # 使用的布局（不需要改）
title:      在macOS中遇到的问题&解决方法 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2020-04-02 				# 时间
author:     moxuy 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - mac
---

>打开破解软件时显示软件已损坏

打开终端，输入以下代码
```
sudo spctl --master-disable
```
然后输入用户密码，完成。

>打开可跨域的chrome

打开终端，输入以下代码
```
open -n /Applications/Google\ Chrome.app/ --args --disable-web-security --user-data-dir=/Users/<yourname>/tmp/chrome
```

>MacOS时间不准

打开终端，输入以下代码
```
sudo sntp -sS time.apple.com
```
