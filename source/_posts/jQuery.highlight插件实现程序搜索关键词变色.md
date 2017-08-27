---
layout: post
title:  "jQuery.highlight插件实现程序搜索关键词变色"
date:   2016-07-27 11:51:05 +0800
categories: notes
author: yangxiangming
tags: [javascript, jquery]
---

插件`jQuery.highlight`可以友好的根据搜索数据的关键词，来遍历搜索结果集数据中的关键词变色，有效提高了用户体验度
<!-- more -->
```javascript
jQuery.fn.highlight = function(c) {
	function e(b, c) {
		var d = 0;
		if (3 == b.nodeType) {
			var a = b.data.toUpperCase().indexOf(c);
			if (0 <= a) {
				d = document.createElement("span");
				d.className = "highlight";
				a = b.splitText(a);
				a.splitText(c.length);
				var f = a.cloneNode(!0);
				d.appendChild(f);
				a.parentNode.replaceChild(d, a);
				d = 1
			}
		} else if (1 == b.nodeType && b.childNodes && !/(script|style)/i.test(b.tagName)) for (a = 0; a < b.childNodes.length; ++a) a += e(b.childNodes[a], c);
		return d
	}
	return this.length && c && c.length ? this.each(function() {
		e(this, c.toUpperCase())
	}) : this
};
jQuery.fn.removeHighlight = function() {
	return this.find("span.highlight").each(function() {
		this.parentNode.firstChild.nodeName;
		with(this.parentNode) replaceChild(this.firstChild, this),
		normalize()
	}).end()
};
```

其中高亮颜色是可以自定义的，需要注意的是定义的颜色名称需要保证和插件`highlight`一致才能起到效果。如下事例

```html
<style>
.highlight{
	color:red;
}
</style>
<script type="text/javascript" src="../js/jquery.min.js"></script>
<script type="text/javascript" src="../js/jQuery.highlight.js"></script>
<script type="text/javascript">
;(function($){
	$(document).ready(function(){
		// 获取关键词并设置数据列表高亮
		$('.list').highlight($('input[name="searchstring"]').val());
	});
}(jQuery));
</script>
<!-- 要搜索的关键词 -->
<input type="text" name="searchstring" value="" /><button type="button">搜索</button>
<!-- 数据列表 -->
<div class="list">
	<ul>
		<li>三月走过 柳絮散落 恋人们匆匆<li>
		<li>我的爱情 纹风不动<li>
		<li>翻阅昨日 仍有温度 蒙尘的心事<li>
		<li>恍恍惚惚 已经隔世<li>
		...
	</ul>
</div>
```
