---
layout:     post   				    # 使用的布局（不需要改）
title:      Vue脚手架 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2018-08-24 				# 时间
author:     moxuy 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - vue
---

## 使用(官方)
```
npm i -g vue-cli            //安装脚手架
vue init webpack my-project //根据webpack模板初始化项目
cd my-project               //进入初始化项目的目录
npm install                 //安装依赖
npm run dev                 //运行项目
```
## 自定义模板
Vue脚手架可以自定义模版，步骤：

1.Fork这个项目https://github.com/vuejs-templates/webpack-simple

2.然后按照自己的喜好去修改，完成后上传到github

3.本地执行
```javascript
var forkName = 刚才Fork过来的项目名,
    gitName = 自己github的代码库名称,
    projectName = 项目名称;
---------------------------------------------------
vue init <gitName + '/' + forkName> projectName
```