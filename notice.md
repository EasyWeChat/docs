title: 模板消息
---

模板消息仅用于公众号向用户发送重要的服务通知，只能用于符合其要求的服务场景中，如信用卡刷卡通知，商品购买成功通知等。不支持广告等营销类消息以及其它所有可能对用户造成骚扰的消息。

关于使用规则，请注意：

    1. 所有服务号都可以在功能->添加功能插件处看到申请模板消息功能的入口，但只有认证后的服务号才可以申请模板消息的使用权限并获得该权限；
    2. 需要选择公众账号服务所处的2个行业，每月可更改1次所选行业；
    3. 在所选择行业的模板库中选用已有的模板进行调用；
    4. 每个账号可以同时使用15个模板。
    5. 当前每个模板的日调用上限为 10 万次【2014年11月18日将接口调用频率从默认的日1万次提升为日10万次，可在MP登录后的开发者中心查看】。

关于接口文档，请注意：

    1. 模板消息调用时主要需要模板 ID 和模板中各参数的赋值内容；
    2. 模板中参数内容必须以 ".DATA" 结尾，否则视为保留字；
    3. 模板保留符号 "{{ }}"。

本 SDK 中由 `Overtrue\Wechat\Notice` 类完成模板消息的功能。

## 获取实例

```php
<?php

use Overtrue\Wechat\Notice;

$appId  = 'wx3cf0f39249eb0e60';
$secret = 'f1c242f4f28f735d4687abb469072a29';

$notice = new Notice($appId, $secret);
```

### API

+ `boolean setIndustry($industryId1, $industryId2)` 修改账号所属行业；
+ `array industries()` 返回所有支持的行业列表，用于做下拉选择行业可视化更新；
+ `string  addTemplate($shortId)` 添加模板并获取模板ID；
+ `int send($to = null, $templateId = null, array $data = array(), $url = null, $color = '#FF0000')` 发送模板消息, 返回消息ID。

非链接调用方法：

```php
$messageId = $notice->send($to, $templateId, array $data, $url, $color);
```


链式调用方法:

    设置模板ID：template / templateId / uses
    设置接收者openId: to / receiver
    设置模板头部颜色：color / topColor
    设置详情链接：url / link / linkTo
    设置模板数据：data / with

    以上方法都支持 `withXXX` 与 `andXXX` 形式链式调用

```php
$messageId = $notice->uses($templateId)->andUrl($url)->withColor($color)->data($data)->send();
// 或者
$messageId = $notice->to($userOpenId)->url($url)->template($templateId)->andData($data)->send();
// 或者
$messageId = $notice->withTo($userOpenId)->withUrl($url)->withTemplate($templateId)->withData($data)->send();
// 或者
$messageId = $notice->to($userOpenId)->color('#ff0000')->url($url)->withTemplateId($templateId)->send();
// ... ...
```

example:

```php
$userId = 'OPENID';
$templateId = 'ngqIpbwh8bUfcSsECmogfXcV14J0tQlEpBO27izEYtY';
$url = 'http://overtrue.me';
$color = '#FF0000';
$data = array(
         "first"    => "恭喜你购买成功！",
         "keynote1" => "巧克力",
         "keynote2" => "39.8元",
         "keynote3" => "2014年9月16日",
         "remark"   => "欢迎再次购买！",
        );

$messageId = $notice->uses($templateId)->withUrl($url)->andData($data)->andReceiver($userId)->send();
```

### 模板数据

为了方便大家开发，我们拓展支持以下格式的模板数据，其它格式的数据可能会导致接口调用失败：

- 所有数据项颜色一样的（这是方便的一种方式）:

    ```php
    $data = array(
        "first"    => "恭喜你购买成功！",
        "keynote1" => "巧克力",
        "keynote2" => "39.8元",
        "keynote3" => "2014年9月16日",
        "remark"   => "欢迎再次购买！",
    );
    ```
  默认颜色为'#173177', 你可以通过 `defaultColor($color)` 来修改

- 独立设置每个模板项颜色的：

    + 简便型：

        ```php
        $data = array(
            "first"    => array("恭喜你购买成功！", '#555555'),
            "keynote1" => array("巧克力", "#336699"),
            "keynote2" => array("39.8元", "#FF0000"),
            "keynote3" => array("2014年9月16日", "#888888"),
            "remark"   => array("欢迎再次购买！", "#5599FF"),
        );
        ```
    + 复杂型（也是微信官方唯一支持的方式，估计没有人想这么用）：

        ```php
        $data = array(
            "first"    => array("value" => "恭喜你购买成功！", "color" => '#555555'),
            "keynote1" => array("value" => "巧克力", "color" => "#336699"),
            "keynote2" => array("value" => "39.8元","color" => "#FF0000"),
            "keynote3" => array("value" => "2014年9月16日", "color" => "#888888"),
            "remark"   => array("value" => "欢迎再次购买！", "color" => "#5599FF"),
        );
        ```

关于模板消息的使用请参考：http://mp.weixin.qq.com/wiki/17/304c1885ea66dbedf7dc170d84999a9d.html