title: Template Message
---

### :zap: Pre-requirements

1. The feature is only enabled for the account which is named as "服务号";
2. The "服务号" account needs certificated by the Wechat offcial;
3. Other mentioned rules please checking the [Wechat Official Wiki](http://mp.weixin.qq.com/wiki/17/304c1885ea66dbedf7dc170d84999a9d.html) (chinese version)

## Usage Memo

The template message is hosted by the `Overtrue\Wechat\Notice` class.

### instance demo

```php
<?php

use Overtrue\Wechat\Notice;

$appId  = 'wx3cf0f39249eb0e60';
$secret = 'f1c242f4f28f735d4687abb469072a29';

$notice = new Notice($appId, $secret);
```

### API

+ `boolean setIndustry($industryId1, $industryId2)`

  > toggle the industry from `$industryId1` to `$industryId2`, once/month
+ `array industries()`

  > Fetching all of supported industries list;
+ `string  addTemplate($shortId)`

  > add the message template and returned the template ID;
+ `int send($to = null, $templateId = null, array $data = array(), $url = null, $color = '#FF0000')`

  > send a template message and returned the message ID;
+ `array getPrivateTemplates()`

  > Fetching all of template list;
+ `array deletePrivateTemplate($templateId)`

  > Delete template by templateId;
  

### None-Chained Method usage:

```php
$messageId = $notice->send($to, $templateId, array $data, $url);
```


### Chained Method usage:

  1. setter the template ID: template / templateId / uses
  2. setter the receiver openId: to / receiver
  4. setter the detail page URL: url / link / linkTo
  5. setter the template data: data / with
  6. send

  all of above are both supported `withXXX` and or `andXXX` method chain

```php
$messageId = $notice->to($userOpenId)->uses($templateId)->andUrl($url)->data($data)->send();
// or
$messageId = $notice->to($userOpenId)->url($url)->template($templateId)->andData($data)->send();
// or
$messageId = $notice->withTo($userOpenId)->withUrl($url)->withTemplate($templateId)->withData($data)->send();
// or
$messageId = $notice->to($userOpenId)->url($url)->withTemplateId($templateId)->send();
// ... ...
```

### example:

```php
$userId     = 'OPENID';
$templateId = 'ngqIpbwh8bUfcSsECmogfXcV14J0tQlEpBO27izEYtY';
$url        = 'http://overtrue.me';
$data = array(
        "first"    => "CHECKOUTED",
        "keynote1" => "Chocolates",
        "keynote2" => "39.8 RMB",
        "keynote3" => "2014/9/16",
        "remark"   => "Have a nice day!",
        );

$messageId = $notice->uses($templateId)->withUrl($url)->andData($data)->andReceiver($userId)->send();
```

### The Template Data

The following data structures were wrapped and tested by this SDK, others should made the RPC call failed Possibly

- all of the columns' colors are sample (simple way):

    ```php
    $data = array(
        "first"    => "CHECKOUTED",
        "keynote1" => "Chocolates",
        "keynote2" => "39.8 RMB",
        "keynote3" => "2014/9/16",
        "remark"   => "Have a nice day!",
    );
    ```
  The default color is '#173177', and it can be reset by the `defaultColor($color)` method;

- setter each of the columns' colors:

    + Fast-way type:

        ```php
        $data = array(
            "first"    => array("CHECKOUTED", '#555555'),
            "keynote1" => array("Chocolates", "#336699"),
            "keynote2" => array("39.8 RMB", "#FF0000"),
            "keynote3" => array("2014/9/16", "#888888"),
            "remark"   => array("Have a nice day!", "#5599FF"),
        );
        ```
    + Official-way types:

        ```php
        $data = array(
            "first"    => array("value" => "CHECKOUTED", "color" => '#555555'),
            "keynote1" => array("value" => "Chocolates", "color" => "#336699"),
            "keynote2" => array("value" => "39.8 RMB","color" => "#FF0000"),
            "keynote3" => array("value" => "2014/9/16", "color" => "#888888"),
            "remark"   => array("value" => "Have a nice day!", "color" => "#5599FF"),
        );
        ```
