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

在计算机科学中，二分查找算法是一种在有序数组中查找某一特定元素的搜索算法。搜索过程从数组的中间元素开始，如果中间元素正好是要查找的元素，则搜索过程结束；如果某一特定元素大于或者小于中间元素，则在数组大于或小于中间元素的那一半中查找，而且跟开始一样从中间元素开始比较。如果在某一步骤数组为空，则代表找不到。这种搜索算法每一次比较都使搜索范围缩小一半。

```php
<?php
/**
 * 二分查找
 */
function binarySearch($target,$object) {
  $total = count($object);
  $lower = 0;
  $high = $total - 1;
  while($lower <= $high) {
    $middle = intval(($lower + $high) / 2);
    if($object[$middle] > $target) {
      $high = $middle - 1;
    } elseif ($object[$middle] < $target) {
      $lower = $middle + 1;
    } else{
      return $middle;
    }
  }
  return false;
}

```

冒泡排序是一种简单的排序算法。它重复地走访过要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端。

```php
<?php
/**
 * 冒泡排序
 */
function bubbleSort($array) {
  $len = count($array);
  for ($i = 0; $i < $len - 1; $i++) {
    for ($j = 0; $j < $len - 1 - $i; $j++) {
      if ($array[$j] > $array[$j+1]) {
        $tmp = $array[$j];
        $array[$j] = $array[$j+1];
        $array[$j+1] = $tmp;
      }
    }
  }
  return $array;
}

```
