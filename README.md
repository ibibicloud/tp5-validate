
## tp5-validate
基于 https://github.com/top-think/think-validate/tree/v2.0.2
如果数据表设计了 <u>__软删除__</u> unique 验证规则 默认数据表排除 <u>__delete_time = 0__</u>
~~~
`delete_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '删除时间',
~~~

### 安装
~~~
composer require ibibicloud/tp5-validate
~~~

### 用法
~~~
use think\facade\Validate;

$validate = Validate::rule([
    'name'  => 'require|max:25',
    'email' => 'email'
]);

$data = [
    'name'  => 'thinkphp',
    'email' => 'thinkphp#qq.com'
];

if ( !$validate->check($data) ) {
    var_dump($validate->getError());
}
~~~

### 自定义 message 错误提示信息
~~~
use think\facade\Validate;

$validate = Validate::rule([
    'name'  => 'require|max:25',
    'email' => 'email'
])->message([
    'name.require'  => '用户名必填',
    'name.max'      => '用户名超出最大长度25',
    'email.email'   => '邮箱格式错误'
]);

$data = [
    'name'  => 'thinkphp',
    'email' => 'thinkphp#qq.com'
];

if ( !$validate->check($data) ) {
    var_dump($validate->getError());
}
~~~

### 支持创建验证器进行数据验证
~~~
<?php
namespace app\index\validate;

use think\Validate;

class User extends Validate
{
    protected $rule =   [
        'name'  => 'require|max:25',
        'age'   => 'number|between:1,120',
        'email' => 'email',    
    ];
    
    protected $message  =   [
        'name.require' => '名称必须',
        'name.max'     => '名称最多不能超过25个字符',
        'age.number'   => '年龄必须是数字',
        'age.between'  => '年龄只能在1-120之间',
        'email'        => '邮箱格式错误',    
    ];
    
}
~~~

验证器调用代码如下：
~~~
$data = [
    'name'  => 'thinkphp',
    'email' => 'thinkphp@qq.com',
];

$validate = new \app\index\validate\User;

if ( !$validate->check($data) ) {
    var_dump($validate->getError());
}
~~~

更多用法可以参考6.0完全开发手册的[验证](https://www.kancloud.cn/manual/thinkphp6_0/1037623)章节