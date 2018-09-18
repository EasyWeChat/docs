## 扫码支付

### 模式一：先生成产品二维码，扫码下单后支付

请务必先熟悉流程：https://pay.weixin.qq.com/wiki/doc/api/native.php?chapter=6_4

#### 生成产品二维码内容

```php
$content = $app->scheme($productId); // $productId 为你的产品/商品ID，用于回调时带回，自己识别即可

//结果示例：weixin://wxpay/bizpayurl?sign=XXXXX&appid=XXXXX&mch_id=XXXXX&product_id=XXXXXX&time_stamp=XXXXXX&nonce_str=XXXXX
```

拿到二维码内容以后，SDK 并不内置二维码生成库，使用你熟悉的工具创建二维码即可，比如 PHP 部分有以下工具可以选择：

- https://github.com/endroid/qr-code
- https://github.com/SimpleSoftwareIO/simple-qrcode
- https://github.com/aferrandini/PHPQRCode

#### 处理回调

当用户扫码时，你的回调接口会收到一个通知，你可以使用下面的代码处理扫码通知：

```php
$app->handleScannedNotify(function($message, $fail, $alert) use ($app) {
  // 1. 被扫码的产品 ID 将会在 $message 里拿到
  // 2. 使用拿到的产品信息，调用微信支付的下单接口创建订单得到 prepay_id 并返回
  $result = $app->order->unify([
      'body' => '腾讯充值中心-QQ会员充值',
      'out_trade_no' => '20150806125346',
      'total_fee' => 88,
      'spbill_create_ip' => '123.12.12.123', // 可选，如不传该参数，SDK 将会自动获取相应 IP 地址
      'notify_url' => 'https://pay.weixin.qq.com/wxpay/pay.action', // 支付结果通知网址，如果不设置则会使用配置里的默认地址
      'trade_type' => 'JSAPI',
      'openid' => 'oUpF8uMuAJO_M2pxb1Q9zNjWeS6o',
  ]);
  
  if (empty($result['prepay_id'])) {
    return $fail('支付失败消息');
  }
  
  return $result['prepay_id']; // 一定要返回 prepay_id
});
```

### 模式二：先下单，生成订单后创建二维码

