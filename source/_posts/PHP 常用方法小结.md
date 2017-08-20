---
layout: post
title:  "PHP 常用方法小结"
date:   2016-06-27 16:53:05 +0800
categories: notes
author: yangxiangming
tags: [php, ip]
---

实际开发过程中，常对IP获取、数组排序、重组、分割以及其他处理等，来实现具体等业务需求，如下方法可以针对二维数组实现不同的重置数据结果
<!-- more -->

```php
<?php
/**
 * 获取客户端IP
 */
function getClientIp(){
  if(isset($_SERVER["HTTP_CLIENT_IP"]) and strcasecmp($_SERVER["HTTP_CLIENT_IP"], "unknown")){
    return $_SERVER["HTTP_CLIENT_IP"];
  }
  if(isset($_SERVER["HTTP_X_FORWARDED_FOR"]) and strcasecmp($_SERVER["HTTP_X_FORWARDED_FOR"], "unknown")){
    return $_SERVER["HTTP_X_FORWARDED_FOR"];
  }
  if(isset($_SERVER["REMOTE_ADDR"])){
    return $_SERVER["REMOTE_ADDR"];
  }
  return "";
}

/**
 * 获取服务端IP
 */
function getServerIp(){
  if (isset($_SERVER)) {
    if($_SERVER['SERVER_ADDR']) {
      $server_ip = $_SERVER['SERVER_ADDR'];
    } else {
      $server_ip = $_SERVER['LOCAL_ADDR'];
    }
  } else {
    $server_ip = getenv('SERVER_ADDR');
  }
  return $server_ip;
}
```

具体`PHP`中`$_SERVER`的用法请参考[$_SERVER](http://php.net/manual/zh/reserved.variables.server.php)

```php
/**
 * 根据二维数组键值进行排序
 */
function arrayMultisSort($array, $key, $sort=SORT_DESC){
  if(is_array($array)){
    // 循环设置键值
    foreach ($array as $val){
      if(is_array($val)){
        $k[] = $val[$key];
      }else{
        return false;
      }
    }
  }else{
    return false;
  }
  // 排序
  array_multisort($k, $sort, $array);
  return $array;
}

/**
 * 保持键值打乱二维数组顺序
 */
function arrayShuffleAssoc($array) {
  if (!is_array($array)) return $array;
  // 获取键值
  $keys = array_keys($array);
  // 打乱键值
  shuffle($keys);
  $random = array();
  // 数组重组
  foreach ($keys as $key){
    $random[$key] = $array[$key];
  }
  return $random;
}

/**
 * 数组分割[类似分页]
 */
function arrayPartPage($num, $page, $array, $order=0){
  // 定全局变量
  global $countpage;
  $page=(empty($page))?'1':$page;
  // 开始位置
  $start=($page-1)*$num;
  if($order==1){
    $array=array_reverse($array);
  }
  // 总数
  $totals=count($array);
  $countpage=ceil($totals/$count);
  $pagedata=array_slice($array, $start, $num);
  return $pagedata;
}

/**
 * 冒泡排序
 */
function arrayBubbleSort($array){
  // 统计总数
  $count = count($array);
  // 双重循环完成，外层控制循环次数当前的最小值。内层控制的比较次数
  for ($i=0; $i < ($count-1); $i++){
    for ($k=($i+1); $k<$count; $k++){
      // 该层对比值的大小直接进行大小替换
      if ($array[$k] < $array[$i]){
        $temp = $array[$i];
        $array[$i] = $array[$k];
        $array[$k] = $temp;
      }
    }
  }
  //返回最终结果
  return $array;
}

/**
 * 选择排序
 */
function arrayChooseSort($array){
  // 双重循环完成，外层控制循环次数当前的最小值。内层控制的比较次数，$i当前最小值的位置，需要参与比较的元素
  for($i=0, $count=count($array); $i<($count-1); $i++) {
    //先假设最小的值的位置
    $p = $i;
    //$k当前都需要和哪些元素比较，$i后边的元素。
    for($k=($i+1); $k<$count; $k++) {
      //$arr[$p] 是 当前已知的最小值
      if($array[$p] > $array[$k]) {
        //比较发现更小的记录下最小值的位置；并且在下次比较时应该采用已知的最小值进行比较。
        $p = $k;
      }
    }
    //已经确定了当前的最小值的位置保存到$p中。如果发现最小值的位置与当前假设的位置$i不同，则位置互换即可
    if($p != $i) {
      $tmp = $array[$p];
      $array[$p] = $array[$i];
      $array[$i] = $tmp;
    }
  }
  //返回最终结果
  return $array;
}

/**
 * 求质数（只能被本身和1整除的数）
 */
function getPrimeNumber($nums=100){
  $result = 0;
  for ($i = 1; $i <= $nums; $i++) {
    $k = 0;
    for ($j = 1; $j < $i; $j++) {
      if ($i % $j == 0) {
        $k++;
      }
    }
    if ($k == 1) {
      $result .= $i.",";
    }
  }
  return $result;
}

/**
 * description 获取毫秒时间
 */
function getMsec($isboor = true) {
    list($usec, $sec) = explode(" ", microtime());
    $microtime = ((float)$usec+(float)$sec);
    list($usec, $sec) = explode(".", $microtime);
    $date = date('Y-m-d H:i:s x', $usec);
    $result = str_replace('x', $sec, $date);
    return $isboor?$result.'Msec':$microtime;
}

/**
 * description 获取设备
 */
function isClient() {
    if(!empty($_SERVER('HTTP_USER_AGENT') && (substr($_SERVER('HTTP_USER_AGENT'), 0, 12) == 'great-winner') || strpos($_SERVER['HTTP_USER_AGENT']), 'great-winner')){
        return true;
    }
    return false;
}

/**
 * 格式化时间
 * param $time 时间戳
 */
function getFormatTime($time){
    date_default_timezone_set('PRC'); //设置东八区时间
    $nowTime = time(); //当前时间
    $result = $temp = 0;
    //时间匹配
    switch ($time) {
        //分钟
        case ($time+60)>$nowTime:
            //$temp = $nowTime-$time;
            //$result = $temp.'秒前';
            $result = '刚刚';
            break;
        //小时
        case ($time+(60*60))>$nowTime:
            $temp = date('i', $nowTime-$time);
            $result = $temp.'分钟前';
            break;
        //一天
        case ($time+(60*60*24))>$nowTime:
            $temp = date('H', $nowTime)-date('H', $time);
            $result = $temp.'小时前';
            break;
        //昨天
        case ($time+(60*60*24*2))>$nowTime:
            $temp = date('H:i', $time);
            $result = '昨天'.$temp;
            break;
        //前天
        case ($time+(60*60*24*3))>$nowTime:
            $temp = date('H:i', $time);
            $result = '前天'.$temp;
            break;
        //7天内
        case ($time+(60*60*24*7))>$nowTime:
            $temp = $nowTime-$time;
            $day = floor($temp/(60*60*24));
            $result = $day.'天前'.date('H:i', $time);
            break;
        //年月日
        default:
            $result = date('Y-m-d', $time);
            break;
    }
    return $result;
}

/**
 * 过滤html中的tag属性
 * param $html 要过滤的数据
 * param $attrs 指定过滤tag属性
 */
function removeAttr($html, $attrs=[]){
    //删除所有属性
    if (!is_array($attrs) or count($attrs) == 0){
        return preg_replace('~<([a-z]+)[^>]*>~i','<$1>', $html);
    } else { //删除部分指定的属性
        foreach($attrs as $attr){
            $regx = '~<([^>]*?)[\s\t\r\n]+('.$attr.'[\s\t\r\n]*=[\s\t\r\n]*([\"\'])[^\3]*?\3)([^>]*)>~i';
            $html = preg_replace($regx,'<$1 $4>', $html);
        }
        return $html;
    }
}

```
