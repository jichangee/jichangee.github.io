---
layout:     post   				    # 使用的布局（不需要改）
title:      在Electron中使用escpos		  # 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2020-08-25 				# 时间
author:     moxuy 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Electron
    - NodeJs
---

## 前言
最近有个需求是需要在收银系统内连接热敏打印机和一体机的客显，因为项目中使用了electron进行开发并发布到Windows，所以就在网上找了下解决方案。
经过一番搜索，发现了一个[node-escpos](https://github.com/song940/node-escpos)库，但是相关开发资料比较少就只能自己摸索了。

## 热敏打印机
这块比较好对接，安装好相关依赖（```escpos```和```escpos-usb```）后，需要使用[electron-rebuild重建](https://github.com/electron/electron-rebuild#how-does-it-work)，因为其中使用到了一个原生模块（```usb```）。

安装完成之后只需按照[例子](https://github.com/song940/node-escpos#example)中操作即可打印出小票

## 客显
对接这块时，一般产品会提供一个escpos指令文档，只要按照文档上的指令给设备发送指令即可，但重要是怎么发送。
在我的项目中使用到了```escpos-serialport```模块。
使用以下代码连接串口
```javascript
const options = {
    baudRate: 2400, // 波特率，一般是这个
    autoOpen: false // 不自动打开
};
const device = new window.escpos.Serial(
    "COM1", // 串口名，根据具体设备来
    { ...options }
);
device.open(() => {
    // 串口连接成功
});
```
连接串口成功之后即可向指定串口发送指令集
```javascript
// 我这台机器的指令文档如下
// 1.ESC  @初始化命令
// ASCII码 格式：ESC  @
// 十进制   格式：[027][064]
// 	十六进制 格式：[1BH][40H]
// 说明：恢复到上电开机时的状态。

// 2.CLR清屏命令
// ASCII码 格式：CLR
// 十进制   格式：[012]
// 十六进制 格式：[0CH]
// 	说明：清除屏幕上的所有字符。

// 3.ESC  Q  A  d1d2d3…dn  CR送显示数据命令
// ASCII码 格式：ESC  Q  A  d1d2d3…dn  CR
// 	十进制   格式：[027][081][065]d1d2d3…dn[013]     48<=dn<=57或dn=45或dn=46
// 	十六进制 格式：[1BH][51H][41H]d1d2d3…dn[0DH]
//                                          30H<=dn<=39H或dn=2DH或dn=2EH
// 	说明：
// a.执行该命令时，会以覆盖模式送要显示的数据，这样就不需要在每次送显示数据前都去执行CAN清除光标行命令了。
// b.显示的d1…dn没有小数点时1<=n<=8。
// c.显示的d1…dn有小数点时1<=n<=15（8位数值+7位小数点）。
// d.显示的内容可用CLR或CAN命令清除。

// 4.ESC  s  n设置 “单价”、“总计”、“收款”、“找零”字符显示状态命令
// ASCII码 格式：ESC  s  n            0<=n<=4
// 	十进制   格式：[027][115] n           48<=n<=52
// 	十六进制 格式：[1BH][73H] n          30H<=n<=34H
// 	说明：(1)当 n=0，四种灯 全暗。
// (2)当 n=1，“单价”灯 亮，其它三种 暗。
// (3)当 n=2，“总计”灯 亮，其它三种 暗。
// (4)当 n=3，“收款”灯 亮。其它三种 暗。
// (5)当 n=4，“找零”灯 亮。其它三种 暗。

device.write(`${String.fromCodePoint(27)}${String.fromCodePoint(64)}`) // 初始化命令，这里需要用到fromCodePoint
device.write(`${String.fromCodePoint(12)}`) // 清屏命令

const price = 123123.1; // 显示的价格（8位数值或者7位数值+小数点）
device.write(`${String.fromCodePoint(27)}${String.fromCodePoint(81)}${String.fromCodePoint(65)}${price}${String.fromCodePoint(13)}`) // 设置价格
```