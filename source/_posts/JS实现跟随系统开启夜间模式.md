---
title: JS实现跟随系统开启夜间模式
date: 2020-02-28
tags:
---
## 最近发现微软和微信公众号的站点可以跟随系统开启夜间模式
如下图
![微软夜间模式.png](https://ww1.sinaimg.cn/large/0068zxMtly1gcbuq7zommj30z30qb7ap.jpg)
在网上搜了一圈发现了如下代码即可判断当前系统是否为夜间模式
```javascript
window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches
```
就这一行代码即可实现跟随系统开启夜间模式，让网站的逼格瞬间提升～