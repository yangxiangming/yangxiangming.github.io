---
layout: post
title: "Git高大上的版本控制器"
date: 2015-09-22 10:32:08 +0800
categories: notes
author: yangxiangming
tag: [git]
type: "categories"
---

`Git`是什么？`Git`能干啥？——`Git`是很牛叉很牛叉的分布式版本控制系统，高效、灵活而且还免费。说白了就是高端大气上档次！是管理项目的不二选择……赶紧搞起来吧！！！
<!-- more -->
#### Git安装
* Window系统安装直接从 [GitBash] 下载，按默认选项安装即可。
* Mac OS X系统安装可以通过homebrew安装Git，参考文档：[brew]

[GitBash]: https://git-for-windows.github.io/
[brew]: http://brew.sh/

#### 配置
* git config –global 配置本地Git库

```bash
#<你的用户名>
$ git config --global user.name "Your Name"

#<你所用邮箱>
$ git config --global user.email "Your@Email.com"

#<依次执行配置用户名和用户邮箱之后，本地Git仓库都会使用这个配置>
```

* 创建非对称RSA加密SSH Key

```bash
$ ssh-keygen -t rsa -C "Your@Email.com"
#<执行命令后会在电脑用户主目录下生成.ssh文件，有id_rsa和id_rsa.pub两个文件不管你用的是GitHub还是用了自己搭配的Git服务器，打
#开id_rsa.pub公钥全选复制，粘贴到Git服务器上面添加Add SSH Key的地方保存，这样本地Git和服务器Git关联起来了。>

#<检测连接是否成功>
$ ssh -T git@github.com
```

#### 本地<=>远程
* git clone 从远程主机克隆一个版本库到本地

```bash
$ git clone <版本库的网址>
$ git clone <版本库的网址> <本地目录名>

#<例如>
$ git clone git@github.com:example/example.git #<这样会在本地主机生成一个目录，与远程主机的版本库同名>

$ git clone git@github.com:example/example.git localExample #<如果这样执行，结果本地版本库和远程的就是不同目录名了>

#<git clone支持多种协议，除了SSH)以外，还支持HTTPS、Git、本地文件协议等>
```

* git fetch 将某个远程主机的更新取回到本地

```bash
$ git fetch <远程主机名>
$ git fetch <远程主机名> <分支名>

#<例如>
$ git fetch origin #<将某个远程主机的更新全部到本地，默认情况下是取回所有分支>

$ git fetch origin example #<指定分支更新到本地，origin主机的example分支>

#<更新到本地之后，可以执行merge合并到本地分支||执行checkout创建新分支都可以>
```

* git pull 取回远程主机某个分支的更新与本地的指定分支合并

```bash
$ git pull <远程主机名> <远程分支名>:<本地分支名>

#<例如>
$ git pull origin example:example #<获取远程origin主机的example分支，与本地的example分支合并。实质上这等同于先执行git fetch再
#做git merge合并>
```

* git push 将本地分支的更新推送到远程主机

```bash
$ git push <远程主机名> <本地分支名>:<远程分支名>

#<例如>
$ git push origin localExample:example #<将本地的localExample分支推送到origin主机的example分支>

$ git push origin example #<如果本地分支名和远程分支名同名可以省略远程分支名。将本地的example分支推送到远程origin主机的
#example分支>
```

在我们使用`git`从远程主机克隆一个版本库的时候，如果克隆到本地的文件夹是空则不会有任何问题，那么当我们克隆到本地文件夹非空时则就会出现错误信息了，并且`clone`不成功

`clone`到本地不为空的文件夹下的时候会出现这样的提示，例如
```bash
yangxiangming@yangxiangmingdeMacBook-Pro:~/workstation$ git clone git@git.coding.net:yangxiangming/yangxiangming.github.io.git yangxiangming.github.io/
fatal: destination path 'yangxiangming.github.io' already exists and is not an empty directory.
```

那么我们怎么解决这种问题，并且使之成功呢！如下操作步骤详解

先进入你想`git clone`到本地的非空文件夹下

```bash
yangxiangming@yangxiangmingdeMacBook-Pro:~$ cd workstation/yangxiangming.github.io/
```

然后执行`git clone`到一个临时文件tmp

```bash
yangxiangming@yangxiangmingdeMacBook-Pro:~/workstation/yangxiangming.github.io$ git clone --no-checkout git@git.github.net:yangxiangming/yangxiangming.github.io.git tmp
Cloning into 'tmp'...
Warning: Permanently added the RSA host key for IP address '60.174.240.191' to the list of known hosts.
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.
Checking connectivity... done.
```

在把`git clone`下来的`.git`移动到当前文件夹下

```bash
yangxiangming@yangxiangmingdeMacBook-Pro:~/workstation/yangxiangming.github.io$ mv tmp/.git .
```

这个时候就可以把第一步`git clone`的临时文件tmp删除掉了

```bash
yangxiangming@yangxiangmingdeMacBook-Pro:~/workstation/yangxiangming.github.io(master+)$ sudo rm -rf tmp
```

最后删除`git`操作痕迹，撤销当前`head`的内容并重置

```bash
yangxiangming@yangxiangmingdeMacBook-Pro:~/workstation/yangxiangming.github.io(master+)$ git reset --hard HEAD
HEAD is now at 8f23e29 Initial commit
yangxiangming@yangxiangmingdeMacBook-Pro:~/workstation/yangxiangming.github.io(master+)$
```

如上操作就可以顺利解决`git clone`不能克隆非空文件夹的问题了，开开心心用起来了，哇哦

Git命令直列举了一部分，还有一些`git merge`、`git status`、`git log`...常用命令等等，具体请参考官方手册[GitBook](https://git-scm.com/book/zh/v2)
