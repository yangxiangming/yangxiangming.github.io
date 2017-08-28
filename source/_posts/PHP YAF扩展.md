---
layout: post
title: "PHP YAF扩展"
date: 2015-11-06 11:02:25 +0800
categories: notes
author: yangxiangming
tags: [php, yaf]
---

`YAF`是鸟哥([惠新宸](http://www.laruence.com/))经典作品之一。`YAF` 扩展框架(全称:Yet Another Framework)
<!-- more -->
YAF 框架，全称 Yet Another Framework，是一个C语言编写的`PHP`框架，是一个以`PHP`扩展形式提供的`PHP开`发框架, 相比于一般的PHP框架, 它更快，更轻便. 它提供了`Bootstrap`, 路由, 分发, 视图, 插件, 既不会有损性能, 又能提高开发效率一个全功能的`PHP`框架。详情请参考官方文档 [YAF](http://www.laruence.com/manual)。

#### 扩展配置
* Windows配置
  * 首先官网下载对应版本的[FAY](https://pecl.php.net/package/yaf)扩展，推荐使用stable稳定版。
  * 解压后复制php_yaf.dll文件到php目录的ext文件中。
  * 编辑php.ini加载yaf扩展，然后重新启动对应的服务。

```bash
......
extension=php_yaf.dll
```

再次访问`phpinfo()`对应的yaf拓展已经加载成功了。

* Mac OS X配置
  * 直接打开Mac OS X功能齐全的终端神器iTerm，运行命令如下

```bash
wget http://pecl.php.net/get/yaf-2.3.5.tgz
tar zxvf yaf-2.3.5.tgz && cd yaf-2.3.5
phpize
./configure --with-php-config=/usr/bin/php-config
make && sudo make install
```

编译安装完成之后，再去ext/php.ini中添加对应的yaf.so扩展，一定要记得重启Apache。

#### 文件目录结构
* 框架目录

```bash
├─ application #框架核心文件夹
      ├─ controllers
      ├─ library
      ├─ models
      ├─ modules
      ├─ plugins
      └─ views
├─ conf #配置参数文件夹
      └─ application.ini
├─ public #入口文件夹
      ├─ css
      ├─ img
      ├─ js
      ├─ .htaccess #apache伪静态
      └─ index.php #入口文件
```
其中的.htaccess文件在用apache的时候设置，当然也可以直接在apache目录中的conf文件添加相应配置也可以。
