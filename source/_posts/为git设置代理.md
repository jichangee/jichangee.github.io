---
layout:     post   				    # 使用的布局（不需要改）
title:      为git设置代理 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2019-12-10 				# 时间
author:     moxuy 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - git
---

```
# 设置git全局代理
git config --global http.proxy 'socks5://127.0.0.1:1080'

git config --global https.proxy 'socks5://127.0.0.1:1080'

# 仅对github设置代理
git config --global http.https://github.com.proxy socks5://127.0.0.1:1080

#取消代理
git config --global --unset http.proxy

git config --global --unset https.proxy
```
端口号需改为本地socks5的监听端口
