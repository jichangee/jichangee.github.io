---
layout:     post   				    # 使用的布局（不需要改）
title:      （翻译）JavaScript — Map vs. ForEach 				# 标题 
# subtitle:   Hello World, Hello Blog #副标题
date:       2017-09-06 				# 时间
author:     薛纪昌 						# 作者
# header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - javascript
---

Array.prototype.map()如果你使用了一段时间的JavaScript，你可能会遇到两个比较相似的数组方法：`Array.prototype.map()` 和 `Array.prototype.forEach()`。

所以，有什么不同呢？

####Map&ForEach的定义
首先让我们看一下在MDN上的定义：
- [forEach()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) —— 给每个数组元素执行一次提供的函数。
- [map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) —— 创建一个新数组，再给每一个新数组元素执行一次提供的函数。

这到底意味着什么？

`forEach()`方法实际上不会返回任何东西(undefined)。它只是简单的在每个数组元素上执行你提供的函数。这个回调函数是被允许改变原有数组的。

`map()`方法也是在每个数组元素上执行你提供的函数。不同的是，`map()`会根据回调函数的返回值来返回一个相同大小的新数组。

#### 代码示例
思考以下数组。如果我们想把每个数组元素乘以二，那`map()`或者`forEach()`都可以使用。

```javascript
let arr = [1, 2, 3, 4, 5];
```

**ForEach：**
你将不会从forEach中得到一个返回值
```javascript
arr.forEach((num, index) => {
    return arr[index] = num * 2;
});
```
**结果：**
```javascript
// arr = [2, 4, 6, 8, 10]
```
**Map:**
```
let doubled = arr.map(num => {
    return num * 2;
});
```
**结果：**
```
// doubled = [2, 4, 6, 8, 10]
```

###速度
jsPerf是一个很好的用来测试不同JavaScript方法和函数执行速度的网站。
这是`forEach()`和`map()`的测试结果：
![](https://www.xuejichang.cn/web/upload/1_aVOlJ0l02ymgVrQ8axIBrQ.png)
正如你所看到的，在我的机器上，`forEach()`要比`map()`慢70%。
你的浏览器可能会不同。你可以[试一下](https://jsperf.com/map-vs-foreach-speed-test)
###功能
如果你喜欢函数式编程，那使用`map()`可能会更好。
这是因为`forEach()`会影响和改变我们的源数组，然而`map()`会返回一个全新的数组——从而保证源数组不变。
###哪一个更好？
这取决于你想实现什么功能。
当你不会改变数组中的数据时，`forEach()`可能会更好，只是想对数组做些什么，比如存入到数据库或者日志中：
```
let arr = ['a', 'b', 'c', 'd'];
arr.forEach((letter) => {
    console.log(letter);
});
// a
// b
// c
// d
```
当要改变数据时，`map()`可能会更好。不仅它更快，而且还会返回新的数组。这意味着我们能做很酷的事情，像链接其他方法（`map()`, `filter()`, `reduce()`, 等等）
```
let arr = [1, 2, 3, 4, 5];
let arr2 = arr.map(num => num * 2).filter(num => num > 5);
// arr2 = [6, 8, 10]
```
上面的代码中，我们首先做的是将数组中每个元素乘以2。在这之后，我们过滤了这个数组，就保存了比5大的元素。最终剩下了`[6, 8, 10]`这个数组`arr2`。

原文：https://codeburst.io/javascript-map-vs-foreach-f38111822c0f