title: 短网址服务
---

本 SDK 由 `Overtrue\Wechat\Url` 提供微信链接转换服务。

主要使用场景： 开发者用于生成二维码的原链接（商品、支付二维码等）太长导致扫码速度和成功率下降，将原长链接通过此接口转成短链接再生成二维码将大大提升扫码速度和成功率。

### 获取实例

```php
<?php

use Overtrue\Wechat\Url;

$appId  = 'wx3cf0f39249eb0e60';
$secret = 'f1c242f4f28f735d4687abb469072a29';

$url = new Url($appId, $secret);
```


## API

+ `string short($url)` 长链接转短链接

example:

```php
$shortUrl = $url->short('http://overtrue.me/open-source');
//
```

微信官方文档：http://mp.weixin.qq.com/wiki/10/165c9b15eddcfbd8699ac12b0bd89ae6.html