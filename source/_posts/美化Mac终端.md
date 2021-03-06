---
layout:     post   				    # 使用的布局（不需要改）
title:      美化Mac终端 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2018-08-27 				# 时间
author:     moxuy 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Mac
    - iTerm
    - ozh
---

## 安装iTerm2
https://www.iterm2.com/

## 安装oh my zsh
https://ohmyz.sh/

在终端中输入
> sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

## 修改iTerm2主题
在[这里](https://github.com/mbadolato/iTerm2-Color-Schemes)有很多主题可供选择，也可以自行上网搜索

下载项目后，在`schemes`目录中选择主题，然后在`iTerm2 - Preferences - Colors - Color Presets... - import` 导入主题即可

## 安装[powerlevel9k](https://github.com/bhilburn/powerlevel9k)

先clone项目
```shell
git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k
```

然后修改iTerm2配置，在 `~/.zshrc` 中找到 `ZSH_THEME` 设置其值为 `"powerlevel9k/powerlevel9k"` ，然后重启，可能会出现乱码，是未安装对应字体造成的。

## 安装字体
### 安装[Meslo](https://github.com/powerline/fonts/blob/master/Meslo%20Slashed/Meslo%20LG%20M%20Regular%20for%20Powerline.ttf)字体。

下载完成后，安装。


然后在 `iTerm2 - Preferences - Text - change font` 中选中安装的字体，重启iTerm2即可。

### 安装其他字体
```
# 在用brew安装字体需要执行该命令
brew tap caskroom/fonts

# 安裝font-sourcecodepro-nerd-font字体
brew cask install font-sourcecodepro-nerd-font

# 或者可以搜索字体，以下搜索nerd相关的字体
brew cask search nerd
```

:-)