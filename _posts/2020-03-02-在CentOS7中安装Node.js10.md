---
layout:     post   				    # 使用的布局（不需要改）
title:      在CentOS 7/Fedora 29/Fedora 28中安装Node.js 10 LTS		  # 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2020-03-02 				# 时间
author:     Kline 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Centos
    - NodeJs
---

要在CentOS 7、Fedora 29、Fedora 28系统中安装Node.js 10.x LTS，请使用NodeSource方式：

```
curl -sL https://rpm.nodesource.com/setup_10.x | bash -
```

上面命令将配置Node.js RPM存储库，下面你需要做的就是安装nodejs包：

1、在CentOS 7系统中，运行以下命令：

```
sudo yum clean all && sudo yum makecache fast

sudo yum install -y gcc-c++ make

sudo yum install -y nodejs
```

2、在Fedora 29、Fedora 28系统中，运行以下命令：

```
sudo dnf install -y gcc-c++ make

sudo dnf install -y nodejs