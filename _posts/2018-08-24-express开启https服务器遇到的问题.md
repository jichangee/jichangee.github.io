---
layout:     post   				    # 使用的布局（不需要改）
title:      express 开启 https服务器遇到的问题 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2018-08-24 				# 时间
author:     薛纪昌 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - express
    - nginx
---

用express写的api服务器，放到CentOS上后出现了问题。具体如下

sudo node index.js 开启监听后，浏览器访问https，会自动断开，而访问http的则可以。

https请求http的接口又会遇到问题，所以只能找解决方法。

log segmentation fault. google和百度了一圈，都没发现解决方法。

最后用Nginx的反向代理猥琐解决了该问题。nginx的安装可以去我的另一篇博客中查看[CentOS环境部署](https://www.xuejichang.cn/blogger.html?id=1)
## 反向代理配置
打开 /etc/nginx/nginx.conf 文件，在监听443端口的服务中加入如下代码
```
location / {
  proxy_pass http://127.0.0.1:3100/;
}
```
这样就把 / 的请求转移到了 127.0.0.1:3100/，然后再由node处理请求，再响应。

也可以配置多条
```
location /manage/ {
  proxy_pass http://127.0.0.1:3100/manage/;
}
```
配置完成之后，需要运行 service nginx restart 来重启Nginx服务。
## 后台运行

使用node index.js开启服务需要ssh一直开着才行，但是我们需要让node能够在后台运行，所以就用到了forever。
```
npm i -g forever
sudo forever start index.js
```
来开启进程守护，让node在后台一直运行。