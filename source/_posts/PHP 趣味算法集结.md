---
layout: post
title:  "PHP 趣味算法集结"
date: 2018-11-08 13:51:05 +0800
categories: notes
author: yangxiangming
tags: [php, fun]
---

PHP作为世界上最好的编程语言...哈哈...。也是可以实现一些算法的，比如杨辉三角、约瑟夫环、扑克洗牌等。
<!-- more -->

洗牌算法是我们常见的随机问题，在玩游戏、随机排序时经常会碰到，本质是让一个数组内的元素随机排列。类似于洗牌，将所有牌的位置打乱，让他们随机出现在任何位置。

```php
<?php
/**
 * 扑克洗牌
 * @param int $cardNum 扑克张数
 */
function washCard($cardNum = 54){
  $cards = $tmp = array();
  for ($i = 0; $i < $cardNum; $i++){
    $tmp[$i] = $i;
  }

  for ($i = 0; $i < $cardNum; $i++){
    $index = rand(0, $cardNum - $i - 1);
    $cards[$i] = $tmp[$index];
    unset($tmp[$index]);
    $tmp = array_values($tmp);
  }
  return $cards;
}

```

> 约瑟夫是1世纪的一名犹太历史学家。他在自己的日记中写道，他和他的40个战友被罗马军队包围在洞中。他们讨论是自杀还是被俘，最终决定自杀，并以抽签的方式决定谁杀掉谁。约瑟夫斯和另外一个人是最后两个留下的人。约瑟夫斯说服了那个人，他们将向罗马军队投降，不再自杀。约瑟夫斯把他的存活归因于运气或天意，他不知道是哪一个。

在计算机编程的算法中，称为约瑟夫环。人们站在一个等待被处决的圈子里。计数从圆圈中的指定点开始，并沿指定方向围绕圆圈进行。 在跳过指定数量的人之后，执行下一个人。 对剩下的人重复该过程，从下一个人开始，朝同一方向跳过相同数量的人，直到只剩下一个人，并被释放。

```php
/**
 * 约瑟夫环
 * @param array $array 数组列表
 * @param int $key 循环到第几剔除
 */
function josephusKing($array, $key, $current = 0){
    $number = count($array);
    $num = 1;
    if(count($array) == 1){
        echo $array[0]."-josephus king";
        return;
    }
    else{
        while($num++ < $key){
            $current++ ;
            $current = $current%$number;
        }
        echo $array[$current]."-cull unit<br/>";
        array_splice($array , $current , 1);
        josephusKing($array , $key , $current);
    }
}

```

杨辉三角中的三角形数表，一堆数字组成的三角形状，是自然界和谐统一的体现。每个数是它左上方和右上方的数的和, 用这样非常简单的规律这样最终可以得到一个无穷无尽的三角形。

```php
/**
 * 杨辉三角
 * @param int $floor 要求的层数
 */
function triangleSequence($floor = 1){
  //初始化数组
  $array = [1,1];
  //初始化索引
  $key = 0;
  while ($key < $floor) {
    if ($key == 0) {
      echo $array[$key]."\t";
    } elseif ($key == 1) {
      echo $array[$key - 1]."\t".$array[$key]."\t";
    } else {
      $old = $array;
      for ($i = 0;$i <= count($old);$i++) {
        $array[$i] = $old[$i-1] + $old[$i];
        echo $array[$i]."\t";
      }
    }
    $key ++;
    echo "<br/>";
  }
}

```
