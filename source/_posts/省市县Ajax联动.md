---
layout: post
title: "省市县Ajax联动"
date: 2016-04-28 12:18:29 +0800
categories: code
author: yangxiangming
tag: [php, ajax, 省市县]
---

不管是Pc端、还是Wap端、或者为客户端提供接口数据，以下`Dome`省市县数据请求以及返回对应的数据处理可以供大家参考试用……
<!-- more -->
当然具体还要以自己面对的具体情况做对应调整……如下基于`yaf`框架作原型事例

Model层逻辑Area.php

```php
<?php
/**
 * description: 地区Model类
 * User: yangxiangming@live.com
 * Date: 2016/4/20
 * Time: 13:35
 */

class AreaModel extends PDO{

    private $tableName;

    public function __construct(){}

    public function getArea($id){
        $sql = "SELECT id, areaName, areaId FROM {$this->tableName} WHERE id=:id";
        $stmt = pdo::db()->prepare($sql);
        return $stmt->execute([':id'=>$id])->fetchAll();
    }
}
```
<div class="mb40"></div>

Model层逻辑Area.php

```php
<?php
/**
 * description: 地区Controller类
 * User: yangxiangming@live.com
 * Date: 2016/4/20
 * Time: 13:52
 */

class areaController extends Yaf_Controller_Abstract {

    public function indexAction(){
        $id = Yaf_Request_Http::getPost('id', 0);
        if(empty($id)){
            exit(json_decode([
                'status'=>0,
                'message'=>'地区id不能为空',
                'data'=>[]
            ]));
        }

        $area = new AreaModel();
        $list = $area->getArea($id);
        exit(json_decode([
            'status'=>1,
            'message'=>'Success',
            'data'=>$list
        ]));
    }
}
```

View层结构及Ajax部署Area.phtml

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Area</title>
</head>
<body>

<label>省/直辖市</label>
<select name="province" id="province">
    <option value="0">请选择</option>
    <?php if(!empty($area)){ foreach($area as $key=>$val){ ?>
    <option value="<?php echo $val['id'];?>"><?php echo $val['areaName'];?></option>
    <?php } } ?>
</select>

<label>市/区</label>
<select name="city" id="city">
    <option value="0">请选择</option>
</select>

<label>镇/乡</label>
<select name="town" id="town">
    <option value="0">请选择</option>
</select>

</body>
</html>

<script type="text/javascript">
    function getArea(v,s){
        $.ajax({
            type:"POST",
            data:$.param({id:v}),
            url:"area/index",
            dataType:'json',
            success:function(data){
                var html = '';
                if(data.status == 1){
                    $.each(function (){
                        html += '<option value="'+data[i].id+'">'+data[i].areaName+'</option>';
                    })
                    $('#'+s).children().eq(0).append(html);
                } else {
                    conlose.log(data.message);
                }
            }
        });
    }

    ;(function($){
        $('select[name="province"],select[name="city"],select[name="town"]').bind('click', function(){
            var v = $(this).val(), s=$(this).attr('id');
            getArea(v,s);
        });
    }(jQuery));
</script>
```

备注：本功能逻辑仅供参考，具体以实际页面结构及对应调试为准。希望对有需求者有所帮助
