---
layout: post
title: "PHPAES加密&解密"
date: 2015-09-25 11:02:25 +0800
categories: code
author: yangxiangming
tags: [php, mcrypt, 加密]
---

`AES`(英文全称英：Advanced Encryption Standard，缩写：`AES`)对称加密算法是目前世界流行的对称加密算法之一，据说美国联邦政府也以这种加密方式为标准，瞬间档次值爆表。
<!-- more -->
> 以PHP为例，请确保PHP配置扩展`mcrypt`已开启，具体填充模式、算法位数、算法Key大小长度等请参考官方手册[Mcrypt](http://php.net/manual/zh/book.mcrypt.php)

```php
<?php
/**
 * Description: MongoDB操作
 * Author: yangxiangming@live.com
 * Date: 2014/12/9
 * Time: 16:35
 */

class McryptAES {
    public static $key = "bcb04b7e103a0cd8b54763051cef08bc55abe029fdebae5e1d417e2ffb2a00a3"; /** 加密KEY */
    public static $mode = MCRYPT_MODE_ECB; /** 填充模式 */
    public static $cipher = MCRYPT_RIJNDAEL_128; /** AES加密算法位数 */

    /**
     * description AES加密
     */
    public static function mcrypt_encrypt_aes($plaintext) {
        $ciphertext = mcrypt_encrypt ( self::$cipher, self::get_hash ( self::$key ), $plaintext, self::$mode, self::get_iv () );
        return base64_encode ( $ciphertext );
    }

    /**
     * description AES解密
     */
    public static function mcrypt_decrypt_aes($ciphertext) {
        $ciphertext = base64_decode ( $ciphertext );
        return trim ( mcrypt_decrypt ( self::$cipher, self::get_hash ( self::$key ), $ciphertext, self::$mode, self::get_iv () ) );
    }

    /**
     * description hash算法生成加密散列值
     */
    public static function get_hash($plaintext) {
        return hash ( 'sha256', $plaintext, true );
    }

    /**
     * description 随机源创建初始向量
     */
    public static function get_iv() {
        $iv_size = mcrypt_get_iv_size ( self::$cipher, self::$mode );
        return mcrypt_create_iv ( $iv_size, MCRYPT_RAND );
    }  
}
// 调用测试
$encrypt = McryptAES::mcrypt_encrypt_aes('develop');
var_dump($encrypt);
$decrypt = McryptAES::mcrypt_decrypt_aes($encrypt);
var_dump($decrypt);
```
一切以实际需求为准，如上例子仅供参考。
