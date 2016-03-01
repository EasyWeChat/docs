title: 企业支付
---

你在阅读本文之前确认你已经仔细阅读了：[微信支付 | 企业付款文档 ](https://pay.weixin.qq.com/wiki/doc/api/mch_pay.php?chapter=14_1)。

## 配置

与支付接口一样，红包接口也需要配置如下参数，需要特别注意的是，红包相关的全部接口**都需要使用 SSL 证书**，因此** cert_path 以及 cert_key 必须正确配置**。

```php
<?php

use EasyWeChat\Foundation\Application;

$options = [
    // payment
    'payment' => [
        'merchant_id'        => 'your-mch-id',
        'key'                => 'key-for-signature',
        'cert_path'          => 'path/to/your/cert.pem',
        'key_path'           => 'path/to/your/key',
        // ...
    ],
];

$app = new Application($options);

$merchantMoney = $app->merchant_pay;
```

## 企业付款

企业付款使用的余额跟微信支付的收款并非同一账户，请注意充值。

### 发送接口

```php
<?php

    $merchantMoneyData = [
            'partner_trade_no' => str_random(8),
            'openid' => 'ogsWRv68KexdBbDR7K9DuDK0sukY',
            'check_name' => 'NO_CHECK',  //文档中三分钟校验实名的方法NO_CHECK OPTION_CHECK FORCE_CHECK
            're_user_name'=>'张三',     //OPTION_CHECK FORCE_CHECK 校验实名的时候必须提交
            'amount' => 100,  //单位为分，不小于300
            'desc' => '企业付款',
            'spbill_create_ip' => '192.168.0.1',  //发起交易的IP地址
        ];
    $result = $merchantMoney->send($merchantMoneyData);

```

> 更多参数请参考官方文档中参数列表。

## 查询付款信息

用于商户对已发放的红包进行查询红包的具体信息以及领取情况 ，普通红包和裂变包均使用这一接口进行查询。

```php
$partner_trade_no = "商户系统内部的订单号（partner_trade_no）";
$merchantMoney->query($partner_trade_no);
```
