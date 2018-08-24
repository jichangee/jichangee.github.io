---
layout:     post   				    # 使用的布局（不需要改）
title:      Cordova+Vue2.0入门笔记 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2018-08-24 				# 时间
author:     薛纪昌 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - vue
    - cordova
    - javascript
---

最近要用cordova+vue开发，在网上找了下两者整合的资料，有很多前辈已经有了教程，所以借鉴过来并且整理一下，加上自己爬的一些坑。

如果还没安装Cordova的，请先安装
## 安装
```
npm i -g cordova
npm i -g vue-cli		// Vue脚手架
```
## 准备
创建一个Cordova项目
```
cordova create cordovaDemo
cd cordova
vue init webpack vueDemo
```
Cordova添加可运行的环境
```
cordova platform add browser     //添加浏览器支持
cordova platform add android     //添加安卓设备支持
cordova platform add ios         //添加ios设备支持
```
Cordova添加通讯录插件，可以调用通讯录api
```
cordova plugin add cordova-plugin-contacts
```
然后打开VueDemo中的index.html文件，在head中加入
```
<script src='cordova.js'></script>
```
这是调用Cordova插件必须要引入的

将webpack输出目录设置到Cordova项目中，打开vueDemo - config - index.js,修改 7，8，9，10行代码，如下
![](http://www.xuejichang.cn/web/upload/cordova.png)
## 开始
然后在vueDemo - src - components 目录下创建一个contact.vue文件，contact中的代码如下（其中的3个function参考w3c）
```html
<template>
  <div class='contact'>
    <a href='javascript:' @click='createContact'>创建联系人</a>
    <a href='javascript:' @click='findContacts'>获取联系人</a>
    <a href='javascript:' @click='deleteContact'>删除联系人</a>
  </div>
</template>
<script>
export default {
  name: 'contact',
  data() {
    return {
    }
  },
  methods: {
    findContacts() {
      var options = new ContactFindOptions();
      options.filter = '';
      options.multiple = true;

      var fields = ['displayName'];
      navigator.contacts.find(fields, contactfindSuccess, contactfindError, options);
      var temp = '';
      function contactfindSuccess(contacts) {
        for (var i = 0; i < contacts.length; i++) {
          temp += contacts[i].displayName + '-' + contacts[i].phoneNumbers.value + '
'
        }
        alert(temp);
      }

      function contactfindError(message) {
        alert('Failed because: ' + message);
      }
    },
    createContact() {
      var myContact = navigator.contacts.create({
        'displayName': 'Test User'
      });
      myContact.save(contactSuccess, contactError);

      function contactSuccess() {
        alert('Contact is saved!')
      }

      function contactError(message) {
        alert('Failed because: ' + message);
      }

    },
    deleteContact() {

      var options = new ContactFindOptions();
      options.filter = 'Test User';
      options.multiple = false;
      var fields = ['displayName'];

      navigator.contacts.find(fields, contactfindSuccess, contactfindError, options);

      function contactfindSuccess(contacts) {

        var contact = contacts[0];
        contact.remove(contactRemoveSuccess, contactRemoveError);

        function contactRemoveSuccess(contact) {
          alert('Contact Deleted');
        }

        function contactRemoveError(message) {
          alert('Failed because: ' + message);
        }
      }

      function contactfindError(message) {
        alert('Failed because: ' + message);
      }

    }
  }
}
</script>
<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
a {
  color: #42b983;
  display: block;
}
</style>
```
然后在 vueDemo - src - router - index.js 文件中添加该组件，（代码如下）
```javascript
import Vue from 'vue'
import Router from 'vue-router'
import Hello from '../components/Hello'
import Contact from '../components/Contact'
Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'Hello',
      component: Hello
    },
    {
    	path: '/contact',
    	name: 'Contact',
    	component: Contact
    }
  ]
})
```
在 VueDemo - src - components - Hello.vue文件中链接路由
```
<router-link to='/contact'>通讯录</router-link>
```
然后在vueDemo目录下打开终端，执行
```
npm run build
cd ../
cordova run android
```
即可在模拟器或真机中运行