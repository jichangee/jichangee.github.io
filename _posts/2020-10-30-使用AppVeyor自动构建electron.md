---
layout:     post   				    # 使用的布局（不需要改）
title:      使用AppVeyor自动构建electron		  # 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2020-10-30 				# 时间
author:     moxuy 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - AppVeyor
    - Electron
---

### 前言
在使用electron原生模块（比如usb）时，MacOS就无法打包Windows版本的安装包，必须要一台Windows电脑才可以。
如果装虚拟机的话，对我这台8G的电脑来说又比较卡顿，所以想使用自动化构建来做这件事情，在网上找了一圈之后，发现AppVeyor可以满足我。

### 授权
我是把代码托管在github上的，所以在AppVeyor中授权github即可，授权之后就会展示账号在github的上的项目，免费版只可以选择一个public项目进行自动构建，不过对于我来说足够了。

### 配置
构建配置只要在项目根目录创建一个```appveyor.yml```文件即可，以下是我的配置
```yml
version:
  v{build}

skip_non_tags: true # 跳过非tag提交构建

platform:
  + x64

cache:
  + node_modules
  + '%APPDATA%\npm-cache'
  + '%USERPROFILE%\.electron'
  + '%USERPROFILE%\AppData\Local\Yarn\cache'

environment:
  nodejs_version: "12.16.3" #指定nodejs版本
  matrix:
  # Windows job
  - job_name: Windows_Build
    appveyor_build_worker_image: Visual Studio 2019

  # Linux job
  - job_name: MacOS_build
    appveyor_build_worker_image: macos

for:
  -
    matrix:
      only:
        - job_name: Windows_Build
    install:
      - ps: Install-Product node $env:nodejs_version
      - npm install
    build_script:
    - ./node_modules/.bin/electron-rebuild.cmd && npm run build:win
  -
    matrix:
      only:
        - job_name: MacOS_build
    install:
      - nvm install 12
      - nvm use 12
      - npm install
    build_script:
    - $(npm bin)/electron-rebuild && npm run build:mac

artifacts:
  - path: 'dist/*.*'
    name: myartifacts
deploy:
  - provider: GitHub
    release: $(APPVEYOR_REPO_TAG_NAME)-test
    auth_token:
      secure: <secure> #在github生成token，并且在AppVeyor中加密
    prerelease: false
    # 多构件环境下需要为true
    force_update: true
    # 发布条件
    tag: $(APPVEYOR_REPO_TAG_NAME)
    on:
      # 设置为true，避免无限循环发布
      APPVEYOR_REPO_TAG: true
    artifact: myartifacts
```

### 构建
如上配置，需要新建一个tag并提交才会触发构建，需要使用到以下指令
```shell
git tag <tag名称> #新建tag
git push origin <tag名称>
```