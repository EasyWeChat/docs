title: 代金券
---

你在阅读本文之前确认你已经仔细阅读了：[微信支付 | 代金券或立减优惠文档 ](https://pay.weixin.qq.com/wiki/doc/api/tools/sp_coupon.php?chapter=12_1)。

## 配置

代金券部分只有发放代金券的接口**才需要使用 SSL 证书**，因此 **cert_path 以及 cert_key 选择性配置**。

## 获取代金券实例

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

$cashCoupon = $app->cash_coupon;
```

## API 列表

### 发放代金券

```php
$result = $cashCoupon->send([
  'coupon_stock_id'  => '1757',
  'partner_trade_no' => '1000009820141203515766',
]);
```

### 查询代金券批次信息

```php
$result = $cashCoupon->queryStock(['coupon_stock_id'  => '1757']);
```

### 查询代金券信息

```php
$result = $cashCoupon->query([
  'coupon_id'  => '1757',
  'openid'     => 'your open id',
  'stock_id'   => '58818',
]);
```
