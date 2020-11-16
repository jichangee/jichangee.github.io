---
layout:     post   				    # 使用的布局（不需要改）
title:      vue服务端渲染（ssr） 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2018-08-24 				# 时间
author:     moxuy 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - vue
    - ssr
---

最近在找关于vue的seo方法，就接触到了服务端渲染这块。

关于vue ssr的基础知识我是在 [这里](https://ssr.vuejs.org/zh/basic.html) 学习的，然后记录一下学习和部署的过程。
## 安装
首先要安装依赖 Vue 和 vue-server-renderer
```
npm install vue vue-server-renderer --save
```
## 开始
创建ssr.js
```javascript
const Vue = require('vue')
const server = require('express')()
const renderer = require('vue-server-renderer').createRenderer()
server.get('*', (req, res) => {
  const app = new Vue({
    data: {
      url: req.url
    },
    template: `<div>访问的 URL 是： {{ url }}</div>`
  })
  renderer.renderToString(app, (err, html) => {
    if (err) {
      res.status(500).end('Internal Server Error')
      return
    }
    res.end(`
      <!DOCTYPE html>
      <html lang='en'>
        <head><title>Hello</title></head>
        <body>${html}</body>
      </html>
    `)
  })
})
server.listen(8080)
```
运行后，可能会响应403，我们只需更改下端口号就行了。

但是这仅仅是静态的数据，还有我们需要引入文件模板
## 引入模板
创建一个模板文件 blogger.template.html
```html
<html>
  <head>
    <!-- 使用双花括号(double-mustache)进行 HTML 转义插值(HTML-escaped interpolation) -->
    <title>{{ title }}</title>
    <!-- 使用三花括号(triple-mustache)进行 HTML 不转义插值(non-HTML-escaped interpolation) -->
  </head>
  <body>
    <div>{{content}}</div>
  </body>
</html>
```
这里直接使用gitbook中的例子，稍作修改

修改ssr.js代码
```javascript
const Vue = require('vue')
const server = require('express')()
const renderer = require('vue-server-renderer').createRenderer()
const fs = require('fs');
const template = fs.readFileSync('./blogger.template.html', 'utf-8')
const conn = require('./mysql')

const app = new Vue({
  data: {
    listTime: [],
    d: {
      content: '',
      title: ''
    }
  },
  template: template
})

server.get('/blogger.html', (req, res) => {
  var id = req.query.id // 获取get参数
  var WHERE = ' WHERE id=' + id
  var sql = 'SELECT * FROM blogger' + WHERE
  conn.query(sql, function (error, result) {
    if (result.length > 0) {
      var data = result[0]
      app.d.title = data.title
      app.d.content = data.content
      renderer.renderToString(app, (err, html) => {
        if (err) {
          res.status(500).end('Internal Server Error')
          return
        }
        res.end(html)
      })
    } else {
      res.status(404).end('404')
    }
  })
})
server.listen(3101)
console.log('http running at 3101...');
```
## 部署
将ssr.js部署到服务器上需要用到[nginx反向代理](https://xuejichang.cn/blogger?id=107)