---
layout:     post   				    # 使用的布局（不需要改）
title:      CentOS环境部署 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2017-04-03 				# 时间
author:     moxuy 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - centos
---

nginx
------------
```
yum install nginx	//安装nginx
service nginx start	//启动nginx服务
wget http://127.0.0.1	//测试nginx服务
```

php 
------------
```
yum install php php-fpm	//安装php
yum install php-mysql php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc	//安装php扩展，可以连接mysql
service php-fpm start	//启动php-fpm服务
cat /etc/php-fpm.d/www.conf |grep -i 'listen ='	//查看php-fpm配置
nginx -t	//查找nginx配置文件
vi /etc/nginx/nginx.conf	//修改nginx配置文件
```
#### 配置如下
```
server {
  listen       80;
  root   /usr/share/nginx/html;
  server_name  localhost;
  #charset koi8-r;
  #access_log  /var/log/nginx/log/host.access.log  main;
  location / {
  	index  index.html index.htm;
  }
  #error_page  404              /404.html;
  # redirect server error pages to the static page /50x.html
  #
  error_page   500 502 503 504  /50x.html;
  location = /50x.html {
  root   /usr/share/nginx/html;
  }
  # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
  #
  location ~ .php$ {
  fastcgi_pass   127.0.0.1:9000;
  fastcgi_index   index.php;
  fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
  include        fastcgi_params;
  }
}
```
```
service nginx restart	//重启nginx服务
wget http://127.0.0.1	//测试nginx服务
```
mysql 
------------
Centos 7.2 安装 Mysql 5.7.13
```
wget http://repo.mysql.com/mysql57-community-release-el7-8.noarch.rpm	//下载mysql的repo源
rpm -ivh mysql57-community-release-el7-8.noarch.rpm  --nodeps --force	//安装mysql57-community-release-el7-8.noarch.rpm包
yum install mysql-server	//安装mysql
service mysqld status	//查看MySQL服务是否已启动
systemctl start mysqld	//启动服务
```
#### 重置root密码
MySQL5.7会在安装后为root用户生成一个随机密码，而不是像以往版本的空密码。 

可以安全模式修改root登录密码或者用随机密码登录修改密码。下面用随机密码方式

MySQL为root用户生成的随机密码通过mysqld.log文件可以查找到：
```
grep 'temporary password' /var/log/mysqld.log
```
修改root用户密码：(MySQL的密码策略比较复杂，过于简单的密码会被拒绝)
```
mysql -u root -p
mysql> Enter password: （输入刚才查询到的随机密码）
mysql> SET PASSWORD FOR 'root'@'localhost'= 'Root-123';
mysql> exit
```
用root新密码登录：
```
mysql -u root -pRoot-123
```
如果上面的方式不能修改可以使用下面安全模式修改root：

关闭服务
```
systemctl stop mysqld.service
vi /etc/my.cnf
```
mysqld下面添加skip-grant-tables 保存退出启动服务
```
systemctl start mysqld.service
mysql -u root //不用密码直接回车

use mysql

update user set authentication_string=password('Root-123') where User='root' and Host='localhost';

flush privileges;

exit;

vi /etc/my.cnf 把 skip-grant-tables 一句删除保存退出重启mysql服务 

systemctl restart mysqld.service
```
再次登录即可
```
mysql -u root -pRoot-123
```
如果进行操作出现下面的提示:

You must reset your password using ALTER USER statement before executing this statement.

就再设置一遍密码
```
set password = password('Root-123');
```
### 开放3306端口
允许使用用户名root密码Root-123456从任何主机连接到mysql服务器
```
mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'Root-123456' WITH GRANT OPTION;
mysql>FLUSH PRIVILEGES;
mysql>exit;
```
开启防火墙mysql 3306端口的外部访问
```
firewall-cmd --zone=public --add-port=3306/tcp --permanent
firewall-cmd --reload
```
安装SSL证书
------------
1.复制证书和KEY到nginx目录下	// /etc/nginx/

2.打开/etc/nginx/nginx.conf,增加代码如下
```
server {

        listen 443;

        server_name www.xuejichang.cn; #填写绑定证书的域名

        ssl on;

        ssl_certificate 1_www.xuejichang.cn_bundle.crt;

        ssl_certificate_key 2_www.xuejichang.cn.key;

        ssl_session_timeout 5m;

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #按照这个协议配置

        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;#按照这个套件配置

        ssl_prefer_server_ciphers on;

        location / {

            root   /usr/share/nginx/html; #站点目录

            index  index.html index.htm;

        }

location ~ .php$ {

  fastcgi_pass   127.0.0.1:9000;

  fastcgi_index   index.php;

  fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;

  include        fastcgi_params;

}

}
```
此时nginx.conf中有两个server

3.在80端口的server中添加代码：
```
rewrite ^(.*) https://$host$1 permanent;	//将站点内的所有网站自动跳转到https链接
```
4.service nginx restart	//重启nginx服务
开启文件共享
------------
```
location /download {#访问地址

autoindex on;   #开启共享

autoindex_exact_size off;	#显示出文件的大概大小，单位是kB或者MB或者GB

autoindex_localtime on;	#显示的文件时间为文件的服务器时间

}
```
去掉url中的html后缀
------------
```
location / {

    //添加上以下代码：

    if (!-e $request_filename){

        rewrite ^(.*)$ /$1.html last;

        break;

    }

}
```

### 安装Node.js 10
```
sudo yum clean all && sudo yum makecache fast

sudo yum install -y gcc-c++ make

sudo yum install -y nodejs
```