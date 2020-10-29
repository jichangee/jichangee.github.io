---
layout:     post   				    # 使用的布局（不需要改）
title:      在centos中使用Jenkins+Gitlab实现自动部署 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2018-08-24 				# 时间
author:     Kline 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - centos
    - jenkins
---

经过一天的摸索与尝试，参考了网上大部分的教程，终于把基本功能搞定了。在Google搜索了很多关键字，都没有找到一篇完整的面向初学者的教程（可能是本人的疏忽），所以借此来记录一下自动部署的流程与踩到的坑。

### 安装

因为Jenkins使用Java编写，所以需要安装java环境

```shell
sudo yum install java
```

安装Jenkins之前要先添加Jenkins库

```shell
wget -O /etc/yum.repos.d/jenkins.repo http://jenkins-ci.org/redhat/jenkins.repo
```

```shell
rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key
```

然后再安装Jenkins

```shell
sudo yum install jenkins
```

安装完成之后启动Jenkins

```shell
sudo service jenkins start
```

Jenkins默认会在8080端口运行，如果8080端口被占用，可以修改Jenkins的配置文件

> /etc/sysconfig/jenkins

修改 `JENKINS_PORT` 的值，然后重启jenkins即可

```shell
sudo service jenkins restart
```

*注意*

阿里云的服务器可能出现jenkins启动成功后无法访问的情况，是因为阿里云对服务器的部分端口进行了限制导致的。建议将jenkins端口修改为 `10000 - 10050` 之间的端口，比如 `10001`。



### 启动配置

浏览器中访问IP:PORT，会出现“解锁”jenkins的界面，进入页面中给出的路径，找到密码，输入到页面中的输入框中，解锁jenkins。

解锁成功后，会出现选择安装插件的界面

![chajian.png](https://i.loli.net/2018/09/27/5bac72f28febd.png)

我们选择【选择插件来安装】，默认会勾选一些插件，我们可以取消选择这些插件来安装我们需要的插件。

搜索【git】并勾选【git】和【gitlab】，然后点击安装。

安装后会跳转到创建用户界面，填写信息并保存完成。

### gitlab hook

进入jenkins管理插件界面，搜索 【hook】，找到gitlab hook勾选安装，完成后重启jenkins。![jenkins-2.png](https://i.loli.net/2018/09/27/5bac72f0756d2.png)

### SSH

在终端输入 `ssh-keygen -t rsa -C 'Your email'` 生成秘钥和公钥。

##### 将秘钥复制到jenkins中

进入Credentials - System - Add domain页面。

![jenkins-3.png](https://i.loli.net/2018/09/27/5bac72f2404d8.png)

在Domain Name中输入`www.gitlab.com`，然后点击OK，之后在左侧菜单中点击`add credentials`。

Kind选择`SSH Username with private key`

Username输入git Name

Private Key 选择`Enter directly`，然后把刚才生成的秘钥输入进去，点击OK

![jenkins-4.png](https://i.loli.net/2018/09/27/5bac740c9ee62.png)

##### 将公钥复制到gitlab中

User - setting - SSH Keys

### 自动构建

在左侧菜单中选择【新建任务】，输入任务名，选择【构建一个自由风格的软件项目】，点击确认。

在【源码管理】栏目，选择【git】，Repository URL输入gitlab项目地址，Credentials选择刚才新建的Credentials。

在【构建触发器】栏目，勾选【Build when a change is pushed to GitLab】，点击【高级】之后，点击【generate】按钮生成token。记录【GitLab webhook URL】和【Secret token 】的值。

在【构建】栏目，选择【增加构建步骤】- 【执行shell】，在【命令】中输入shell构建代码。

点击保存。

### 构建shell

如果顺利的话，向gitlab提交代码，会触发jenkins的构建代码。在触发构建代码之前，jenkins会先clone项目代码，按照默认配置，代码会clone在`/var/lib/jenkins/workspace`。

我使用的web服务器是 Nginx，web根目录在`/usr/share/nginx/html`。

我的部署shell代码比较简单粗暴（不会~）。

```shell
# 进入jenkins工作目录
cd /var/lib/jenkins/workspace
# 删除web服务目录文件
rm -r -f /usr/share/nginx/html/jenkins/workspace/gitlab-test
# 拷贝jenkins工作目录中的代码到web服务目录
cp -R gitlab-test /usr/share/nginx/html/jenkins/workspace
```

### webhooks

还有关键的一步，就是设置gitlab的webhooks。

进入gitlab项目 - setting - Integrations

将刚才配置构建时保存的`GitLab webhook URL`和`Secret Token`填入保存，然后点击Test，测试。

如果成功，页面会出现提示 `HTTP 200`

如果失败，需要到jenkins管理界面 - 系统管理 - 全局安全设置 - CSRF Protection中，取消勾选【防止跨站点请求伪造】即可。

### 获取root权限

为jenkins获取root权限
```shell
gpasswd -a root jenkins
```
修改`/etc/sysconfig/jenkins`文件
```
JENKINS_USER=root
JENKINS_GROUP=root
```
重启jenkins
```shell
service jenkins restart
```

### npm command not found

再构建中执行shell时，会提示`npm command not found`
可以在插件管理中安装`nodejs`插件
系统管理 - 全局工具配置 - nodejs中配置保存
然后在项目构建配置中勾选node
