title: 二维码
---

本 SDK 由 `Overtrue\Wechat\QRCode` 提供微信二维码创建与获取等服务。

目前有2种类型的二维码：

1. 临时二维码，是有过期时间的，最长可以设置为在二维码生成后的7天（即604800秒）后过期，但能够生成较多数量。临时二维码主要用于帐号绑定等不要求二维码永久保存的业务场景
2. 永久二维码，是无过期时间的，但数量较少（目前为最多10万个）。永久二维码主要用于适用于帐号绑定、用户来源统计等场景。

### 获取实例

```php
<?php

use Overtrue\Wechat\QRCode;

$appId  = 'wx3cf0f39249eb0e60';
$secret = 'f1c242f4f28f735d4687abb469072a29';

$qrcode = new QRCode($appId, $secret);
```


## API

+ `Bag temporary($sceneId, $expireSeconds = null)` 创建临时二维码；
+ `Bag forever($sceneValue)` 创建永久二维码
+ `Bag card(array $card)` 创建卡券二维码
+ `string show($ticket)` 获取二维码网址，用法： `<img src="<?php $qrcode->show($qrTicket); ?>">`；
+ `void download($ticket, $filename)` 下载二维码到本地，`$filename` 为带文件名的目标路径；

example:

创建临时二维码:

```php
$result = $qrcode->temporary(56, 6 * 24 * 3600);

$ticket = $result->ticket;// 或者 $result['ticket']
$expireSeconds = $result->expire_seconds; // 有效秒数
$url = $result->url; // 二维码图片解析后的地址，开发者可根据该地址自行生成需要的二维码图片
```

创建永久二维码:

```php
$result = $qrcode->forever(56);// 或者 $qrcode->forever("foo");

$ticket = $result->ticket; // 或者 $result['ticket']
$url = $result->url;
```
下载二维码到本地：

```php
$qrcode->download($ticket, __DIR__ . '/code.jpg');
```
