---
layout: post
title:  "PHP 趣味算法集结"
date: 2018-11-08 13:51:05 +0800
categories: notes
author: yangxiangming
tags: [php, fun]
---

PHP作为世界上最好的编程语言...哈哈...。也是可以实现一些算法的，比如杨辉三角、约瑟夫环、扑克洗牌、冒泡、二分等。
<!-- more -->

```php
<?php
/**
 * 二分查找
 */
 function binsearch($target,$object){
  $total=count($object);
  $lower=0;
  $high=$total-1;
  while($lower<=$high){
    $middle=intval(($lower+$high)/2);
    if($object[$middle]>$target){
      $high=$middle-1;
    } elseif($object[$middle]<$target){
      $lower=$middle+1;
    } else{
      return $middle;
    }
  }
  return false;
}


```
