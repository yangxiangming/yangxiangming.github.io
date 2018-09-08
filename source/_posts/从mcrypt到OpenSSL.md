---
layout: post
title: "从mcrypt到OpenSSL"
date: 2018-09-08 10:54:25 +0800
categories: code
author: yangxiangming
tags: [php, mcrypt, OpenSSL, 加密]
---

`mcrypt`扩展已经大约10年之久了，从`php7.1+`以后它将被从核心代码中移除并且移到PECL中。
<!-- more -->
直接会被`OpenSSL`所取代，程序宝宝再也不用担心AES、DES等加密，具体请参考`php`手册[OpenSSL](http://php.net/manual/zh/ref.openssl.php)

```php
/**
 * @desc OpenSSL加密算法
 */
class OpenSSL {

	private static $key = "bcb04b7e103a0cd8b54763051cef08bc55abe029fdebae5e1d417e2ffb2a00a3";

	/**
	 * @desc 验证获取加密算法方式
	 */
	private static function getMethods($cipher=null){
		if ($cipher && in_array($cipher, openssl_get_cipher_methods())) {
			return $cipher;
		} else {
			/**
			 * 为空返回 AES-128-ECB 加密算法方式
			 */
			$cipher = openssl_get_cipher_methods();
			return $cipher[5];
		}
	}

	/**
	 * @desc 数据加密
	 */
	public static function encrypted($data, $key=null, $cipher=null){
		if (empty($data)) {
			return false;
		}

		if (empty($cipher)) {
			$cipher = self::getMethods();
		}

		//初始化向量
		$ivlen = openssl_cipher_iv_length($cipher);
    	$iv = openssl_random_pseudo_bytes($ivlen);

    	//加密 - 版本判断
    	if(phpversion() >= '7.1.0'){
			$ciphertext = openssl_encrypt($data, $cipher, $key, $options=0, $iv);
    	} else {
    		$ciphertext = openssl_encrypt($plaintext, $cipher, $key, $options=OPENSSL_RAW_DATA, $iv);
    	}
    	return $ciphertext;
	}

	/**
	 * @desc 数据解密
	 */
	public static function decrypted($data, $key=null, $cipher=null){
		if (empty($data)) {
			return false;
		}

		if (empty($cipher)) {
			$cipher = self::getMethods();
		}

		//初始化向量
		$ivlen = openssl_cipher_iv_length($cipher);
    	$iv = openssl_random_pseudo_bytes($ivlen);

    	//解密 - 版本判断
    	if(phpversion() >= '7.1.0'){
    		$plaintext = openssl_decrypt($data, $cipher, $key, $options=0, $iv);
    	} else {
    		$plaintext = openssl_decrypt($ciphertext_raw, $cipher, $key, $options=OPENSSL_RAW_DATA, $iv);
    	}

    	return $plaintext;
	}
}
```
