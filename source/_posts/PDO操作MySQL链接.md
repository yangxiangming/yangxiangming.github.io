---
layout: post
title: "PDO操作MySQL数据库"
date: 2015-09-07 10:42:15 +0800
categories: code
author: yangxiangming
tag: [php, pdo]
---

`PDO`操作`MySQL`数据库实例-连接数据库，`PDO`扩展为`PHP`访问数据库定义了一个轻量级的、一致性的接口，它提供了一个数据访问抽象层，这样，无论使用什么数据库，都可以通过一致的函数执行查询和获取数据。
<!-- more -->
```php
<?php
/**
 * Description: PDO数据库操作
 * Author: yangxiangming@live.com
 * Date: 2014/7/29
 * Time: 13:35
 */

class PDOSAFE extends PDO {
  /**
   * description 定义私有静态变量
   */
  private static $safepdo;

  /**
   * description 构造函数
   */
  private function __construct() {
  }

  /**
   * description 实例化调用PDO链接数据库
   */
  private function pdolink() {
      try {
          self::$safepdo = new PDO ( BASE_TYPE . ':host=' . BASE_HOST . ';dbname=' . BASE_NAME, BASE_USER, BASE_PASS, array (
                  PDO::ATTR_PERSISTENT => TRUE,
                  PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
                  PDO::MYSQL_ATTR_INIT_COMMAND => 'SET NAMES UTF8'
          ) );
          return self::$safepdo;
      } catch ( Exception $e ) {
          throw $e;
      }
  }

  /**
   * description 覆盖__clone()方法，禁止克隆
   */
  private function __clone() {
  }

  /**
   * description 单例模式，实例化调用数据库链接
   */
  public static function calldb() {
      if (self::$safepdo == null) {
          self::$safepdo = self::pdolink ();
      }
      return self::$safepdo;
  }

  /**
   * description 获取键值
   */
  public static function getkeys($fields) {
      try {
          $keys = implode ( '`, `', array_keys ( $fields ) );
          return '`' . $keys . '`';
      } catch ( Exception $e ) {
          throw $e;
      }
  }

  /**
   * description 获取预处理值
   */
  public static function getvalues($fields) {
      try {
          $keys = implode ( ', :', array_keys ( $fields ) );
          return ':' . $keys . '';
      } catch ( Exception $e ) {
          throw $e;
      }
  }

  /**
   * description 绑定对应键值
   */
  public static function setkeys($fields) {
      try {
          $string = '';
          foreach ( $fields as $key => $value ) {
              $string .= $key . '=:' . $key . ',';
          }
          return substr ( $string, 0, - 1 );
      } catch ( Exception $e ) {
          throw $e;
      }
  }
}
```

简述单例模式事例
* 单例模式的构造函数必须是私有的不能被外部实例化的，关键字：private
* 单例模式还要设置一个能被外部访问的公有静态方法，关键字：public
* 单例模式还需要定义一个能保存类实例化的静态变量

```php
<?php
/**
 * Description: PDO数据库操作
 * Author: yangxiangming@live.com
 * Date: 2014/12/9
 * Time: 16:05
 */
 class Sample {
   static public $instance; // 静态变量保存类中的实例

   private function __construct(){} // 防止外部实例化

   static public function getInstance(){ // 公共静态方法检测是否有实力对象
     if(!self::$instance){
       self::$instance = new self();
     }
     return self::$instance;
   }
 }

```
