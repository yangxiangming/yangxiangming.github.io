---
layout: post
title:  "Welcome to Blog Generate!"
date:   2015-10-17 13:53:35 +0800
categories: notes
author: yangxiangming
tags: [jekyll, 博客]
---
`Jekyll`是一个简单免费的Blog生成工具，它可以不需要数据库的支持，生成静态文件，还可以免费的部署到`Github`上面，还可以绑定自己的域名，哇塞！是不是感觉很棒是不是感觉很高大上^_^.还等什么赶紧搞一个。
<!-- more -->
如下以Mac OS系统微操作背景，生成目录命令如下

```bash
$ gem install jekyll #安装jekyll
$ jekyll new yangxiangming.github.io #创建jekyll目录blog
```

当然如果想在本地运行，继续执行命令如下

```bash
$ cd yangxiangming.github.io #进入到blog目录
$ jekyll serve #启动jekyll服务
# => Now browse to http://localhost:4000
```
打开浏览器直接访问`http://localhost:4000`即可。具体详情可参考[Jekyll](http://jekyllrb.com/docs/home)

`Jekyll`静态博客如果文章数量太多没有分页实在对于我们有强迫症的人就是精神的折磨，好在`Jekyll`支持数据分页。开启`Jekyll`分页模式

配置文件`_config.yml`添加分页配置项

```config
paginate: 6
paginate_path: "page:num"
```

设置文章列表页`index.html`(一般默认都是`index.html`)

```html
<ul class="post-list">
  { for post in paginator.posts }
    <li>
      <h2>
        <a class="post-link" href="{ post.url | prepend: site.baseurl }">{ post.title }</a>
      </h2>
      { post.excerpt }
      <hr/>
      <span class="post-meta">Posted on: { post.date | date: "%b %-d, %Y" } ｜ Author: { post.author }</span>
    </li>
  { endfor }
</ul>

{ if paginator.total_pages > 1 }
<div class="pagination">
  <span class="total-page">Total { paginator.total_pages } Page</span> |
  { if paginator.previous_page }
    <a href="{ paginator.previous_page_path | prepend: site.baseurl | replace: '//', '/' }">&laquo; Prev</a>
  { else }
    <span>&laquo; Prev</span>
  { endif }

  { for page in (1..paginator.total_pages) }
    { if page == paginator.page }
      <em>{ page }</em>
    { endif }
  { endfor }

  { if paginator.next_page }
    <a href="{ paginator.next_page_path | prepend: site.baseurl | replace: '//', '/' }">Next &raquo;</a>
  { else }
    <span>Next &raquo;</span>
  { endif }
</div>
{ endif }
```
请注意如上`code`有些括号和％忘记加了，具体可根据自己的列表需求结合分页方法作调整。详情请参考官方文档 [Pagination](https://jekyllrb.com/docs/pagination/)
