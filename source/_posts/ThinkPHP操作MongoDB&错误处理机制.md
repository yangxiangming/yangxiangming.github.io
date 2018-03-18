---
layout: post
title: "MongoDB操作&错误处理机制"
date: 2015-09-10 17:02:18 +0800
categories: code
author: yangxiangming
tag: [php, mognodb, thinkphp]
---

`Thinkphp`对`Mongdb`的支持依赖于`PHP`对`Mongodb`的支持，如果出现类似“系统不支持:`mongoClient`”的提示，说明你的`PHP`还没支持`Mongodb`扩展。配置就不介绍了`php.ini`……
<!-- more -->
Model文件：ExampleModel.class.php

```php
<?php
/**
 * Description: MongoDB操作
 * Author: yangxiangming@live.com
 * Date: 2015/9/9
 * Time: 13:35
 */

namespace Bbsapi\Model;
use Think\Model\MongoModel;

class ExampleModel extends MongoModel {

}
```
Controller文件：ExampleController.class.php

```php
<?php
/**
 * Description: MongoDB操作
 * Author: yangxiangming@live.com
 * Date: 2015/9/9
 * Time: 13:51
 */

namespace \Controller;
use Think\Model\ExampleModel;
class ExampleController extends ExampleModel{

    public function example(){
        $where['_id'] = '54dd9116e4b061818991ac7d';
        $model = D('Example');

        /** 查询 */
        $result = $model->where($where)->select();

        /** 添加 */
        $data['name'] = 'Example';
        ……
        $model->add($data);

        /** 更新 */
        $data['name'] = 'ExampleTmp';
        ……
        $model->where($where)->save($data);

        /** 删除 */
        $model->where($where)->delete();
    }
}
```
一切需要实践才知道可不可行，赶紧去试试吧！

`TP`框架开发项目的过程中经常会在调试中可能输入的URL有误导致浏览器报错，而非我们通常所设置的`404`那样人性化显示……
例如错误

```bash
:）非法操作:example
错误位置
FILE: /server/website/.../ThinkPHP/Library/Think/Controller.class.php 　LINE: 170
TRACE
#0 /server/website/.../ThinkPHP/Library/Think/Controller.class.php(170): E('\xE9\x9D\x9E\xE6\xB3\x95\xE6\x93\x8D\xE4\xBD\x9C:exa...')
#1 [internal function]: Think\Controller->__call('example', '')
#2 /server/website/.../ThinkPHP/Library/Think/App.class.php(172): ReflectionMethod->invokeArgs(Object(Bbsapi\Controller\EmptyController), Array)
#3 /server/website/.../ThinkPHP/Library/Think/App.class.php(194): Think\App::exec()
#4 /server/website/.../ThinkPHP/Library/Think/Think.class.php(120): Think\App::run()
#5 /server/website/.../ThinkPHP/ThinkPHP.php(97): Think\Think::start()
#6 /server/website/.../index.php(37): require('/server/website...')
#7 {main}
```
这个时候可能我们开发不需要做什么处理，但是难保用户或者好奇心者给你乱输URL让你的项目报警，这就需要我们自己进行处理了。

引导用户访问不存在的Controller或Function，在Controller文件创建EmptyController.class.php

```php
<?php
/**
 * Description: 空操作404等错误
 * Author: yangxiangming@live.com
 * Date: 2015/11/05
 * Time: 16:35
 */  

use Think\Controller
class EmptyController extends Controller {

    public function index() {  
        $this->_empty();  
    }  

    public function _empty() {  
        header('HTTP/1.1 404 Not Found');  
        $this->display('Public:404');  
    }  
}
```

在Lib文件创建EmptyAction.class.php控制器

```php
<?php
/**
 * Description: 空操作404等错误
 * Author: yangxiangming@live.com
 * Date: 2015/11/05
 * Time: 16:40
 */  

class EmptyAction extends CommonAction {  
   public function _initialize(){  
        parent::_initialize();  
   }  

   public function index() {  
       $this->_empty();  
   }  

   public function _empty() {  
       header('HTTP/1.1 404 Not Found');  
       $this->display('Public:404');  
   }  
}
```
以上是TP访问URL错误处理，具体还是以实际遇到的错误来实现不同的引导，当然也可以自定定义的更人性化……
