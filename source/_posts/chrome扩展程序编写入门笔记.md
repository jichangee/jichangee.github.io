---
layout:     post   				    # 使用的布局（不需要改）
title:      chrome扩展程序编写入门笔记 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2018-08-24 				# 时间
author:     moxuy 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - chrome
---

>chrome扩展程序时用来向chrome添加或者修改功能的浏览器扩展程序。

比如Adblock Plus就是一款十分著名的chrome扩展程序，用来屏蔽页面上的一些广告，也包括部分视频网站的视频广告。

如果自己编写一款扩展程序也是十分简单的，尤其是对于前端的同学来说~

chrome扩展程序实际上就是 HTML + CSS + JavaScript ，还有一个JSON文件：manifest.json

这篇文章就以获取猫眼中前十条热门影片的例子来展开，有如下功能。
![chrome-extend.gif](https://i.loli.net/2018/09/27/5bac72f35e5d0.gif)

## 准备
创建一个maoyan目录，进入该目录后创建如下文件和目录

- maoyan

 | - images   // 存放图片文件

 | - scripts   // 存放js文件

 | - styles    // 存放css文件

 | - popup.html    // 单机图标时显示的页面

 | - tab.html    // 新建tab页时显示的页面
 
## manifest 

这个文件起主要作用，来告诉chrome不同文件对应的不用功能
比如这四个字段，这四个字段为必填
```
{
  'manifest_version': 2,   // manifest版本号，须填写为2
  'name': 'maoyan',        // 扩展程序名
  'description': 'maoyan', // 扩展程序描述
  'version': '0.0.1',      // 扩展程序版本号
}
```
还有很多重要的字段可以到 [扩展开发文档](http://open.chrome.360.cn/extension_dev/manifest.html) 中查阅，360从Google直接翻译过来了，不过版本比较老。
## 开始
因为是在chrome新建标签页中显示，所以我们需要用到
```
'chrome_url_overrides': {
  'newtab': 'tab.html'
}
```
为新建标签页指定显示tab.html页面，此时，我们就可以在tab.html中编写代码了。

## 调试
首先进入 [chrome扩展程序管理页面](chrome://extensions/)

勾选顶部的 开发者模式

此时就会出现三个按钮，分别是【加载已解压的扩展程序】【打包扩展程序】【立即更新扩展程序】

我们单机【加载已解压的扩展程序】，然后选择我们创建的目录，就会在扩展程序列表中出现一个maoyan扩展程序。

这时我们新建一个标签页看看，是不是出现了gif中所示的页面啦~

修改代码后直接刷新tab页即可。
## 打包
单击【打包扩展程序】

首次可不选密钥文件，打包之后会生成一个密钥文件，之后再打包就必须引入这个密钥文件了。

打包后生成crx文件即可拖入chrome扩展程序管理页面中安装。
