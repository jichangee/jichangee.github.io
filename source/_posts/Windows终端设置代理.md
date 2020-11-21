---
title: Windows终端设置代理
date: 2020-11-3
tags:
---
### PowerShell设置代理
```shell
$env:http_proxy="http://127.0.0.1:1086"
$env:https_proxy="http://127.0.0.1:1086"
```

### cmd设置代理
```shell
set HTTP_PROXY=http://127.0.0.1:1086
set HTTPS_PROXY=http://127.0.0.1:1086
```