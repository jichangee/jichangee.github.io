---
layout:     post   				    # 使用的布局（不需要改）
title:      屏蔽Chrome“请停用开发者模式运行的扩展程序”的提示 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2018-08-24 				# 时间
author:     moxuy 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - chrome
---

该方法只适用于Windows

1.下载Chrome策略组模板 链接: https://pan.baidu.com/s/1boMMC2b 密码: rdsk

2.打开Windows策略组，Win+R，运行gpedit.msc。

3.【计算机配置】->【管理模板】右击【添加/删除模板】，添加刚才下载的adm文件。

4.添加成功后，会在【管路模板】->【经典管理模板】下出现【Google】，打开【Google】->【Google Chrome】(不是推荐的那个文件夹)，选中后在右边的设置中找到【扩展程序】并双击。

5.双击打开【配置扩展程序安装白名单】->【已启用】->选项 -> 【显示】，在打开的窗口中输入需要加入白名单的扩展程序ID（扩展程序必须先打包成crx文件，拖入chrome安装）