

```php
$config = array(
    'APP_ID' => 'appID',
    'APP_SECRET' => 'appsecret',
    'TEMPLATE_ID' => '默认模板ID',
    // 一次舔多人,最多可以舔100个人,模板最多只能10个
    'user' => array(
        array(
            // name可看做是备注
            'name' => '001',
            'id' => '被推送人的微信号',
            // 这里是开始恋爱的日期，格式必须是 2017-01-01
            'date' => '2017-01-01',
            // 城市只定位到市级即可
            'city' => '潍坊',
            // 这里是对方的生日日期，格式必须是 05-20
            'birthday' => '05-20',
            // 每个编号可对应一个模板,为空则使用默认模板ID
            'TEMPLATE_ID' => '早安呀的模板ID',
        ),
        // 这里是多用户写法
        // array(
        //     'name' => '002',
        //     'id' => '被推送人的微信号',
        //     'date' => '2017-01-01',
        //     'city' => '德州',
        //     'birthday' => '05-20',
        //     'TEMPLATE_ID' => '',
        // ),
    ),
    // 用来推送是否成功
    'MASTER_ID' => '主人的微信号',
    'MASTER_TEMPLATE_ID' => '推送提醒的模板ID',
);
```