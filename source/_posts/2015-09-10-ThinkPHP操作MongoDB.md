---
layout: post
title: "MongoDB操作"
date: 2015-09-10 17:02:18 +0800
categories: code
author: yangxiangming
tag: [php, mognodb, thinkphp]
---

`Thinkphp`对`Mongdb`的支持依赖于`PHP`对`Mongodb`的支持，如果出现类似“系统不支持:`mongoClient`”的提示，说明你的`PHP`还没支持`Mongodb`扩展。配置就不介绍了`php.ini`……
<!-- more -->
> Model文件：ExampleModel.class.php

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
> Controller文件：ExampleController.class.php

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
