---
layout:     post   				    # 使用的布局（不需要改）
title:      Windows PowerShell配置		  # 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2020-10-29 				# 时间
author:     薛先森 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Windows
    - PowerShell
---

最近一直在使用Windows开发，所以需要配置一下终端。
在Windows store中安装Terminal，因为这个版本的界面看起来比较舒服，如下。
![](https://i.loli.net/2020/10/29/HyiR4A3dUfqXrT6.png)

然后就是安装```oh-my-posh & posh-git```
```
Install-Module -Name PSReadLine -AllowPrerelease -Force # PSReadLine
Install-Module posh-git -Scope CurrentUser # posh-git
Install-Module oh-my-posh -Scope CurrentUser # oh-my-posh
```
安装完成之后可能会有乱码，需要安装字体，我安装的是[Fira Code](https://github.com/tonsky/FiraCode/releases)，下载解压之后，把ttf中的字体全选，然后右击为所有用户安装即可。
然后在terminal中设置```"fontFace" : "Fira Code Retina"```。