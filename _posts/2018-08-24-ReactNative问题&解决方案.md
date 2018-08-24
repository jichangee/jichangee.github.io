---
layout:     post   				    # 使用的布局（不需要改）
title:      ReactNative问题&解决方案 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2018-08-24 				# 时间
author:     薛纪昌 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - reactnative
    - javascript
---

## 调试
### Android 
将手机连接至电脑，并且开启USB调试功能，然后在shell中输入
```
react-native run-android
```
等待apk传输完成后，进入应用，可通过摇动手机来打开开发者菜单
依次选择 Dev Settings  -->  Debug server host

输入主机IP和端口号（一般是8081）

然后再通过摇动手机，选择Reload即可。

###Debug
>Native module VectorIconsModule tried to override VectorIconsModule for module name RNVectorIconsModule.
If this was your intention, set canOverrideExistingModule=true

1. 进入目录：android->app->src->main->java.... 
2. 打开MainApplication.java文件
3. 查看@ Override下的代码是否有重复，有就删除重复的那一行

可能的原因：执行了多次react-native link。