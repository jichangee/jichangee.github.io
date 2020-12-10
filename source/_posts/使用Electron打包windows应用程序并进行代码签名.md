title: 使用Electron打包windows应用程序并进行代码签名
author: Moxuy
date: 2020-12-10 14:30:07
tags:
---
分为两种证书签名方式
- PFX软证书
- Ukey证书

PFX比较简单，我使用的是electron-builder打包，在```electron-builder.json```文件中写入配置
```json
  "win": {
    "icon": "resources/favicon.ico",
    "target": [{ "target": "nsis", "arch": ["ia32"] }],
    "artifactName": "xxx.${ext}",
    "verifyUpdateCodeSignature": false,
    "signingHashAlgorithms": [
      "sha256",
      "sha1"
    ],
    "signDlls": true,
    "rfc3161TimeStampServer": "http://timestamp.digicert.com",
    "certificateFile": "xxx.pfx",
    "certificatePassword": "xxx"
  }
```
其中```rfc3161TimeStampServer```是时间戳服务器Url，```certificateFile```是使用makecert制作的证书导出后的文件，具体操作如下：
- 需要在Windows环境下安装Visual Studio，去官方网站上下载个community版本的就好了
- 安装完成后在开始菜单中找到VS目录，打开里面的cmd，就可以使用makecert命令了，输入```makecert -r -pe -n "cn=MyCA" -$ commercial -a sha1 -cy authority -ss my```，设置密码
- 在MMC的证书管理中导出pfx，Win + R打开运行，输入MMC打开证书管理单元
	- 开始→运行→MMC，打开一个空的MMC控制台。
	- 在控制台菜单，文件→添加/删除管理单元→添加按钮→选”证书”→添加→选”我的用户账户”→关闭→确定
    - 在控制台菜单，文件→添加/删除管理单元→添加按钮→选”证书”→添加→选”计算机账户”→关闭→确定
    - 然后导出为PFX格式的证书，PKCS#12规范的证书，包含了公钥和私钥，导出时需要提供一个私钥的保护密码，在导出时设置即可
> 制作步骤转至https://blog.csdn.net/popozhu/article/details/5793923