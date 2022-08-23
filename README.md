# 利用微信服务号实现早安自动化

## 功能简介

- 支持100人群发/10个模板(不可能所有的宝都叫一个名字吧)，舔一个人叫舔，舔一百个人叫…
- 实时通知推送成功/失败数量和具体名字

## 效果预览

注：推送提醒 仅主人才会收到

![image](https://user-images.githubusercontent.com/31686695/186155177-3957ede4-21a2-4a8d-8add-5b681afc4b47.png)


## 食用方法

### 申请测试号

前往 `微信公众平台` 申请 `接口测试帐号` ，因为只有 `服务号` 有 `模板消息` 推送功能， `订阅号`没有（ `服务号` 需要 `企业` 才能申请

`https://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=sandbox/login`

### 获取appID和appsecret

登录过后我们得到一个测试号，我们需要保存的信息 `appID` 和 `appsecret`

![image](https://user-images.githubusercontent.com/31686695/186154966-621fa612-5645-4127-bd35-3983b7d4eb48.png)

### 获取微信号

假设我们们要推送给张三，让张三扫描左侧的 `测试号二维码` 关注，右侧会列出 `微信号` ，我们把需要推送的用户的 `微信号` 保存下来

![image](https://user-images.githubusercontent.com/31686695/186155009-99f7b90e-cdd8-46d5-aab1-5a9a27961cd6.png)


### 获取模板ID

我们在 `模板消息接口` 中点击 `新建测试模板` ，`模板标题` 即为推送卡片的标题，这里博主填写为 `早安呀~` ， `模板内容` 填入如下内容

```
今日天气: {{weather.DATA}}
当前温度: {{temp.DATA}}
今日温度: {{tempRange.DATA}}
今天是我们的第 {{loveDay.DATA}} 天
距离你的生日还有 {{birthDay.DATA}} 天
{{rainbow.DATA}}	
```

![image](https://user-images.githubusercontent.com/31686695/186155021-5f8de82e-4f06-4555-b0e6-bdc7d42ab72d.png)

我们再次重复上面的步骤，`模板标题` 填写 `推送提醒` ， `模板内容` 填入如下内容

```
共推送 {{count.DATA}} 人
成功: {{successNum.DATA}} | 失败: {{errorNum.DATA}}
成功用户: {{success.DATA}}
失败用户: {{error.DATA}}	
```

这样我们就得到了两个 `模板ID` ，保存下来。

![image](https://user-images.githubusercontent.com/31686695/186155054-3d198c0e-587a-41b7-90d2-2342c6ea07ce.png)

## 填写配置

目前的得到的信息清单如下，直接对照着填入下面的 `php` 代码中即可

```
appID,appsecret,微信号,两个模板ID
```

---
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

## 定时推送

我们可以把上面的 `php` 文件上传到网站中，使用宝塔计划任务进行触发，或者说写个 `shell` 脚本，`python` 脚本之类的搭配 `crontab` 定时触发

![image](https://user-images.githubusercontent.com/31686695/186155115-c75e8030-6af7-4dc3-b504-2e621714559a.png)
