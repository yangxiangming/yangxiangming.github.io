---
layout: post
title: "<A>target属性的新开窗口问题"
date: 2016-05-11 16:18:29 +0800
categories: notes
author: yangxiangming
tags: [html, a]
---

不管在PC端还是在Wap端浏涉及到的A连接中的`target`属性(规定在何处打开`href`连接)，以下特别说明新窗口打开问题(`target＝”_blank”`)。
<!-- more -->
* target＝”_blank” 正常状态新窗口打开，每次来点击都会新窗口打开。
* 没有下划线或者前面或者后面有个空格，这种时候你再来点击就会出现问题，当你第一次点击该连接可以正常新窗口打开，但是当你回到该页面再来点击，就不会在新窗口打开了，只会去刷新你第一次点击打开过的那个窗口。

```html
<a target="_blank" href="http://www.yangxiangming.com/">Target develop</a>
<a target=" _blank" href="http://www.yangxiangming.com/">Target develop</a>
<a target="_blank " href="http://www.yangxiangming.com/">Target develop</a>
<a target="blank" href="http://www.yangxiangming.com/">Target develop</a>
```
