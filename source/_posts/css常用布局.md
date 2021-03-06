---
layout:     post   				    # 使用的布局（不需要改）
title:      css常用布局 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2018-08-24 				# 时间
author:     moxuy 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - css
---

>css常用的布局有：双飞翼布局、圣杯布局、flex布局。

## 水平垂直居中
```css
.block{
	position: fixed;
	height: 100px;
	width: 100px;
	background-color: #000;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	margin: auto;
}
或者
.block{
	position: fixed;
	height: 100px;
	width: 100px;
	background-color: #000;
	top: 50%;
	left: 50%;
	margin: -50px 0 0 -50px;
}
```

## 圣杯布局
左右两列宽度固定，中间DIV宽度自适应。
![sbbj.png](https://i.loli.net/2018/09/27/5bac740b31210.png)
Demo：https://codepen.io/jichangee/pen/vpwNqO

#### 准备
1. 一个父元素div，设置类名为container。
2. 三个子元素div，分别为middle、left、right。
3. header和footer

#### 开始
我们需要让middle的内容最先显示出来，所以要把middle放在三个div的最前面，然后是left，再是right。
设置middle、left、right的float为left，使三个div可以显示在同一行。
```css
.container div{
	height: 200px;
}
.container .left,
.container .right,
.container .middle{
	float: left;
}
.container .left,
.container .right{
	width: 200px;
}
```
因为middle是要求自适应的，所以可以设置middle的width：100%。
```css
.middle{
	width: 100%;
}
```
设置之后，middle会把left和right挤到下一行去。
然后需要用到负边距来把这两个div再重新移到和middle同一行去。
负边距只要能大于div的宽度，就可以让div移到上一行。
left需要在middle同一行的最左边，所以可以设置left的margin-left: -100%;
right需要在middle同一行的最右边，所以可以设置right的margin-left: -200px;
```css
.container .left{
 	margin-left: -100%;
}
.container .right{
	margin-left: -200px;
}
```
都移上去了之后还没完，因为这时的left和right会把middle挡住，所以要先把middle的宽度缩小。
我们可以设置container的内左右边距为left和right的宽度。
```css
.container{
	padding: 0 200px;
}
```
设置之后，middle的左右宽度会各缩小200px，left和right也跟着缩进了。
然后我们只需要把left和right移回到原来的位置就可以了。
给left和right设置position: relative;，再分别设置left: -200px;和right: -200px;
```css
.container .left,
.container .right{
	position: relative;
}
.container .left{
	left: -200px;
}
.container .right{
	right: -200px;
}
```
再设置下header和footer的样式
```css
header,footer{
  height: 50px;
  text-align: center;
  background-color: #333;
  color: #fff;
}
```
到这里，圣杯布局就完成了。

## 双飞翼布局

Demo: https://codepen.io/jichangee/pen/ZvNOYe

其实双飞翼布局和圣杯布局差不多，只是解决【left和right挡住middle内容】的思路不一样而已。
先回到这一步，我们给middle增加一个子元素content，然后设置margin: 0 200px;
```css
.content{
	margin: 0 200px;
}
```