---
layout:     post   				    # 使用的布局（不需要改）
title:      Git学习笔记 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2018-08-24 				# 时间
author:     moxuy 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - git
    - 笔记
---

### 设置Git用户名和邮箱
```
git config --global user.name 'Your Username'
git config --global user.email 'email@example.com'
```
### 创建本地git仓库
```
mkdir <dirname>		//创建目录
git clone <.git>		//克隆远程项目到本地
```
### 更新代码
```
git fetch		//更新本地代码到最新的版本
git merge		//将更新后的代码合并到当前分支
```
### 分支
```
git branch		//查看分支
git checkout -b <branch name>		//创建并切换到分支
git add . 		//添加全部已修改的文件到暂存区
git commit -m <'comments'>		//提交更改
git checkout master		//切换回主分支
git merge <branch name>		//将指定分支合并到主分支
git branch -d <branch name>		删除指定分支
```
### 版本回退
```
git log		//查看详细的历史版本情况
git log --pretty=oneline		查看简略的历史版本情况
git reset --hard HEAD^		//^回退一个版本，^^回退两个版本，以此类推
git reset --hard <ID>		/根据版本ID回退（版本ID在git log中可以查看），输入ID的前几位即可
```
### SSH Key
```
ssh-keygen -t rsa -C 'email'		//在本地生成SSH Key
```