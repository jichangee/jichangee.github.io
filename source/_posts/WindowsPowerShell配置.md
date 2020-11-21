---
title: Windows PowerShell配置
date: 2020-10-29 20:45:26
tags:
---

最近一直在使用Windows开发，所以需要配置一下终端。
在Windows store中安装Terminal，因为这个版本的界面看起来比较舒服，如下。
![](https://i.loli.net/2020/10/29/HyiR4A3dUfqXrT6.png)

然后就是安装```oh-my-posh & posh-git```
```
Install-Module -Name PSReadLine -AllowPrerelease -Force # PSReadLine
Install-Module posh-git -Scope CurrentUser # posh-git
Install-Module oh-my-posh -Scope CurrentUser # oh-my-posh
```
安装完成之后可能会有乱码，需要安装字体，我安装的是[Fira Code](https://github.com/tonsky/FiraCode/releases)，下载解压之后，把ttf中的字体全选，然后右击为所有用户安装即可。
然后在terminal中设置```"fontFace" : "Fira Code Retina"```。

### 启动配置
输入```notepad.exe $Profile```，即可编辑启动配置，以下是我的配置

```shell
Import-Module posh-git # 引入 posh-git
Import-Module oh-my-posh # 引入 oh-my-posh

Set-Theme Paradox # 设置主题为 Paradox

Set-PSReadLineOption -PredictionSource History # 设置预测文本来源为历史记录
 
Set-PSReadlineKeyHandler -Key Tab -Function Complete # 设置 Tab 键补全
Set-PSReadLineKeyHandler -Key "Ctrl+d" -Function MenuComplete # 设置 Ctrl+d 为菜单补全和 Intellisense
Set-PSReadLineKeyHandler -Key "Ctrl+z" -Function Undo # 设置 Ctrl+z 为撤销
Set-PSReadLineKeyHandler -Key UpArrow -Function HistorySearchBackward # 设置向上键为后向搜索历史记录
Set-PSReadLineKeyHandler -Key DownArrow -Function HistorySearchForward # 设置向下键为前向搜索历史纪录

$env:http_proxy="http://127.0.0.1:1086"
$env:https_proxy="http://127.0.0.1:1086"

curl ip.sb
```