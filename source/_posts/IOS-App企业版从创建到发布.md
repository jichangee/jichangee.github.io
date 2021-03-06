---
title: IOS-App企业版从创建到发布
date: 2019-12-21
tags:
    - IOS
    - Ad Hoc
---
## 最近把一个iOS App用企业签名之后发布了，记录一下。

### 首先要制作Ad Hoc的profile文件
登录[苹果开发者](https://developer.apple.com/account)之后，进入Certificate
![Certificate](https://i.loli.net/2019/12/21/82HEJtr3PwB4F5D.jpg)
点击+号
![image.png](https://i.loli.net/2019/12/21/2n1KFCQNSeUJ4Hf.png)
选择下图红框内的选择，点击continue
![image.png](https://i.loli.net/2019/12/21/iqgfWBnL8TEMtQF.png)
这时，需要在本地创建一个Certificate并上传。
打开钥匙串访问，点击右上角菜单，选择证书助理，然后选择从证书颁发机构请求证书
![image.png](https://i.loli.net/2019/12/21/2fMRzspTmPSJDtZ.png)
填入信息，选择保存到磁盘，这样就会在本地生成一个证书，然后上传。
![image.png](https://i.loli.net/2019/12/21/fXBzJSEolMjyC6x.png)
上传完成之后就可以下载Certificate了。
下载证书完成之后，双击打开，然后就可以在钥匙串访问app中找到，然后右击导出，设置密码即可。

### 添加测试设备
因为Ad Hoc类型的ipa安装包只能安装在测试设备之上，所以我们需要先添加测试设备。
选择Devices，然后点击+号
![image.png](https://i.loli.net/2019/12/21/XvlN5ycGDZMz41i.png)
填入设备名称和设备UDID即可，设备UDID可在[这里](https://www.pgyer.com/tools/udid)获取。

### 添加App ID
选择Identifiers，然后点击+号
![image.png](https://i.loli.net/2019/12/21/f57kMXFWRHJ3bKS.png)
选择App IDs，点击continue。

#### Description
随意添加app相关的

#### Bundle ID 
为app的包名，自己填写

#### Capabilities 
勾选
Associated Domains（微信分享会用到）和
Push Notifications（通知推送）
这两个比较常用，可以先勾选

全部填写完成之后，点击continue，然后register

### 申请profile
选择Profiles，然后点击+号
![image.png](https://i.loli.net/2019/12/21/u3U5do8yNcBFb6X.png)
选择Distribution类目下的Ad Hoc，然后点击continue。

App ID下拉框中找到刚才添加的App ID，点击continue。

这一步就是选择测试设备了，可将刚才添加的测试设备勾选上，之后打包后即可安装在测试设备，无需签名。点击continue

然后自己给profile起一个文件名，再单击Generate即可。

## 总结
至此，Ad Hoc需要的文件就准备齐全，即可打包生成ipa。