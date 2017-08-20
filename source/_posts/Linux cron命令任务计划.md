---
layout: post
title:  "Linux cron命令任务计划"
date:   2016-08-12 14:06:05 +0800
categories: notes
author: yangxiangming
tags: [linux, cron]
---

`Cron`是一个强大的任务计划程序，支持 `Unix/Linux` 等其他的操作系统。它可以通过设定的时间运行命令实现任务脚本自动化，有效提高开发人员以及系统运维人员的工作效率
<!-- more -->
`cron` 是一个可以用来根据时间、日期、月份、星期的组合来调度对重复任务的执行的守护进程，要使用 `cron` 服务，你必须安装了 `vixie-cron RPM` 软件包，而且必须在运行 `crond` 服务。
* 要判定该软件包是否已安装，使用 `rpm -q vixie-cron` 命令。
* 要判定该服务是否在运行，使用 `/sbin/service crond status` 命令。

> `Cron` 的主配置文件是 `/etc/crontab`，它的格式是：

```bash
minute  hour    day   month   dayofweek command
<分钟>  <小时>  <日>  <月份>  <星期>  <命令>
```

* minute #分钟，从 0 到 59 之间的任何整数                                                                     
* hour #小时，从 0 到 23 之间的任何整数                                                                     
* day #日期，从 1 到 31 之间的任何整数（如果指定了月份，必须是该月份的有效日期）                           
* month #月份，从 1 到 12 之间的任何整数（或使用月份的英文简写如 `jan、feb` 等等）                           
* dayofweek #星期，从 0 到 7 之间的任何整数，这里的 0 或 7 代表星期日（或使用星期的英文简写如 `sun、mon` 等等）  
* command #要执行的命令（命令可以是 `ls /proc >> /tmp/proc` 之类的命令，也可以是执行你自行编写的脚本的命令。）

要启动 `cron` 服务，使用 `/sbin/service crond start` 命令。要停止该服务，使用 `/sbin/service crond stop` 命令。

> `Crontab`常用命令操作

```bash
crontab file #用指定的文件替代目前的crontab。
crontab- #用标准输入替代目前的crontab.
crontab-1 #列出用户目前的crontab.
crontab-e #编辑用户目前的crontab.
crontab-d #删除用户目前的crontab.
crontab-c dir- #指定crontab的目录
```

> `Crontab`任务简单事例

```
#每天凌晨0点执行
00 00 * * * /server/php7.02/php /htdocs/example.web.com/action/post/push.php

#每10分钟执行一次
*/10 * * * * /server/php7.02/php /htdocs/example.web.com/action/post/total.php

#每小时执行
0 * * * * /server/php7.02/php /htdocs/example.web.com/action/post/example.php

#每天执行
0 0 * * * /server/php7.02/php /htdocs/example.web.com/action/post/example.php

#每周执行
0 0 * * 0 /server/php7.02/php /htdocs/example.web.com/action/post/example.php

#每月执行
0 0 1 * * /server/php7.02/php /htdocs/example.web.com/action/post/example.php

#每年执行
0 0 1 1 * /server/php7.02/php /htdocs/example.web.com/action/post/example.php
```
