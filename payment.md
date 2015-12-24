title: 支付
---

你在阅读本文之前确认你已经仔细阅读了：[微信支付|商户平台开发文档](https://pay.weixin.qq.com/wiki/doc/api/index.html)。

## 配置

配置在前面的例子中已经提到过了，支付的相关配置如下：

```php
<?php

use Overtrue\WeChat\Application;

$options = [
    // payment
    'payment' => [
        'merchant_id'        => 'your-mch-id',
        'cert_path'          => 'path/to/your/cert.pem',
        'key_path'           => 'path/to/your/key',
        // 'device_info'     => '013467007045764',
        // 'sub_app_id'      => '',
        // 'sub_merchant_id' => '',
        // ...
    ],
];

$app = new Application($options);

$payment = $app['payment'];
```

## 创建订单

```php
<?php

use EasyWeChat\Payment\Order;

$attributes = [
    'body'             => 'iPad mini 16G 白色',
    'detail'           => 'iPad mini 16G 白色',
    'out_trade_no'     => '1217752501201407033233368018',
    'total_fee'        => 5388,
    'notify_url'       => 'http://xxx.com/order-notify', // 支付结果通知网址
    // ...
];

$order = new Order($attributes);

```

通知url必须为直接可访问的url，不能携带参数。示例：notify_url：“https://pay.weixin.qq.com/wxpay/pay.action”

## 下单接口

### 刷卡支付

[官方文档](https://pay.weixin.qq.com/wiki/doc/api/micropay.php?chapter=9_10)

```php
$result = $payment->pay($order);
```

## 统一下单

[公众号支付](https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=9_1)、[扫码支付](https://pay.weixin.qq.com/wiki/doc/api/native.php?chapter=6_1)、[APP 支付](https://pay.weixin.qq.com/wiki/doc/api/app.php?chapter=9_1) 都统一使用此接口完成订单的创建。

```php
$result = $payment->prepare($order);
$prepareId = $result->prepay_id;
```

## 支付结果通知

TODO

## 撤销订单API

目前只有 **刷卡支付** 有此功能。

> 调用支付接口后请勿立即调用撤销订单API，建议支付后至少15s后再调用撤销订单接口。

```php
$orderNo = "商户系统内部的订单号（out_trade_no）";
$payment->reverse($orderNo);
```

或者：

```php

$orderNo = "微信的订单号（transaction_id）";
$payment->reverseByTranscationId($orderNo);
```


## 查询订单

该接口提供所有微信支付订单的查询，商户可以通过该接口主动查询订单状态，完成下一步的业务逻辑。

需要调用查询接口的情况：

  - 当商户后台、网络、服务器等出现异常，商户系统最终未接收到支付通知；
  - 调用支付接口后，返回系统错误或未知交易状态情况；
  - 调用被扫支付API，返回USERPAYING的状态；
  - 调用关单或撤销接口API之前，需确认支付状态；

```php
$orderNo = "商户系统内部的订单号（out_trade_no）";
$payment->query($orderNo);
```

或者：

```php

$orderNo = "微信的订单号（transaction_id）";
$payment->queryByTranscationId($orderNo);
```

## 关闭订单

> 注意：订单生成后不能马上调用关单接口，最短调用时间间隔为5分钟。

```php
$orderNo = "商户系统内部的订单号（out_trade_no）";
$payment->close($orderNo);
```

## 申请退款

当交易发生之后一段时间内，由于买家或者卖家的原因需要退款时，卖家可以通过退款接口将支付款退还给买家，微信支付将在收到退款请求并且验证成功之后，按照退款规则将支付款按原路退到买家帐号上。

注意：

> 1、交易时间超过一年的订单无法提交退款；
> 2、微信支付退款支持单笔交易分多次退款，多次退款需要提交原支付订单的商户订单号和设置不同的退款单号。一笔退款失败后重新提交，要采用原来的退款单号。总退款金额不能超过用户实际支付金额。

```php
$result = $payment->refund($orderNo, 100); // 总金额 100， 退款 100，操作员：商户号
// or
$result = $payment->refund($orderNo, 100, 80); // 总金额 100， 退款 80，操作员：商户号
// or
$result = $payment->refund($orderNo, 100, 80, 1900000109); // 总金额 100， 退款 80，操作员：1900000109
```

## 查询退款

提交退款申请后，通过调用该接口查询退款状态。退款有一定延时，用零钱支付的退款20分钟内到账，银行卡支付的退款3个工作日后重新查询退款状态。

```php
$result = $payment->queryRefund($outTradeNo);
// or
$result = $payment->queryRefundByTranscationId($transactionId);
// or
$result = $payment->queryRefundByRefundNo($outRefundNo);
// or
$result = $payment->queryRefundByRefundId($refundId);
```

## 下载对账单

```php
$bill = $payment->downloadBill('20140603'); // type: ALL
// or
$bill = $payment->downloadBill('20140603', 'SUCCESS'); // type: SUCCESS
```

第二个参数为类型：

 - **ALL**：返回当日所有订单信息（默认值）
 - **SUCCESS**：返回当日成功支付的订单
 - **REFUND**：返回当日退款订单
 - **REVOKED**：已撤销的订单

## 测速上报

```php
$payment->report($api, $timeConsuming, $resultCode, $returnCode);
// or
$payment->report($api, $timeConsuming, $resultCode, $returnCode, [
        'err_code'     => 'xxxx',
        'err_code_des' => '...',
        'out_trade_no' => '...',
        'user_ip'      => '...',
    ]);
```

## 转换短链接

```php
$shortUrl = $payment->urlShorten('http://easywechat.org');
```

## 授权码查询OPENID接口
TODO