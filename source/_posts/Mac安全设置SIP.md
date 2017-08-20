---
layout: post
title: "Mac 安全设置SIP"
date: 2015-11-20 09:58:25 +0800
categories: notes
author: yangxiangming
tag: [mac, os x, sip]
---

Mac不久前更新了系统，升级到 `OS X 10.11`，在编辑安装软件扩展的时候遇到了很蛋疼的问题，老是`make`失败。
<!-- more -->
经过Google终于找到问题所在，原来在`OS X El Capitan`中有一个跟安全相关的模式叫`SIP(System Integrity Protection)`，它能够禁止让
软件以`root`身份来在Mac上运行，在升级到`OS X 10.11`中或许你就会看到部分应用程序被禁用了，这些或许是你通过终端或者第三方软件源安
装。对于大多数用户来说，这种安全设置很方便，但是对于我们这些屌丝程序猿不需要这样的设置了啊！下面是关闭 Mac SIP 安全设置：

> 重启 Mac，按住`Command+R`键直到 Apple logo 出现就可以进入`Recovery Mode`
  找到工具选择终端（`Utilities > Terminal`），输入`csrutil disable`回车（这样 SIP 安全设置就被关闭）

```bash
[-bash-3.2# csrutil disable
Successfully disabled System Integrity Protection. Please restart
the machine for the changes to take effect.
-bash-3.2#
```

> 然后重启 Mac 就可以了。如果要开启重复如上步骤输入`csrutil enable`即可。
