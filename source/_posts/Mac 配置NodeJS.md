---
layout: post
title:  "Mac 配置Node.JS"
date:   2016-06-08 11:33:35 +0800
categories: notes
author: yangxiangming
tags: [mac, os x, node.js]
---

`Node.JS`是什么？简单来说就是运行在服务度的`JavaScript`.(其实我觉得`Node.JS`就是一企图妄想取代我大`PHP`地位的不安前端分子...当然开玩笑的了)
<!-- more -->
`Node.JS`是一个基于`Chrome JavaScript`运行的平台，基于`Google`的`V8`引擎，`V8`引擎执行`JavaScript`的速度非常快，性能非常好。如果你是一个前端程序员，你不懂得像`PHP`、`Python`或`Ruby`等动态编程语言，然后你想创建自己的服务，那么`Node.js`是一个非常好的选择。

`Mac OS X`配置安装`Node.JS`使用`brew`安装方便快捷

```bash
brew install node
```

安装完成之后，来测试一下是否执行成功，直接执行`node -v`查看对应版本号即可。然后创建对应测试文件Test.js。

```bash
var http = require('http');

http.createServer(function (request, response) {

	#<设置头部信息发送 HTTP 头部;HTTP 状态值: 200 : OK;内容类型: text/plain>
	response.writeHead(200, {'Content-Type': 'text/plain'});

	#<发送响应数据 "Hello Node">
	response.end('Hello Node\n');
}).listen(8888);

#<终端输出如下信息>
console.log('Server running at http://127.0.0.1:8888/');
```

然后中断执行，通过浏览器访问`http://127.0.0.1:8888/`就可以看到Hello Node了

```bash
node Test.js
```

顺便提一下`brew`的安装和使用，`brew`全称`Homebrew`是`Mac OS X`不可或缺的套件管理器。直接执行如下代码即可

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
安装好之后，以后再想安装任何开发所需的软件／拓展／插件／配置等都可以`brew install 你要安装的项`

比如我上面安装`Node.JS`的时候，执行`brew install node`，如下自动下载安装是不是很方便

```bash
yangxiangming@yangxiangmingdeMacBook-Pro:~$ brew install node
==> Downloading https://homebrew.bintray.com/bottles/node-6.0.0.el_capitan.bottle.tar.gz
######################################################################## 100.0%
==> Pouring node-6.0.0.el_capitan.bottle.tar.gz
==> Caveats
Please note by default only English locale support is provided. If you need
full locale support you should:
  `brew reinstall node --with-full-icu`

Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
  /usr/local/Cellar/node/6.0.0: 3,655 files, 38.8M
yangxiangming@yangxiangmingdeMacBook-Pro:~$
```
