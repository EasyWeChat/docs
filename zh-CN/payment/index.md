# 支付


你在阅读本文之前确认你已经仔细阅读了：[微信支付 | 商户平台开发文档](https://pay.weixin.qq.com/wiki/doc/api/index.html)。

## 配置

配置在前面的例子中已经提到过了，支付的相关配置如下：

```php
use EasyWeChat\Factory;

$options = [
    // 前面的appid什么的也得保留哦
    'app_id'             => 'xxxx',
    'merchant_id'        => 'your-mch-id',
    'key'                => 'key-for-signature',
    'cert_path'          => 'path/to/your/cert.pem', // XXX: 绝对路径！！！！
    'key_path'           => 'path/to/your/key',      // XXX: 绝对路径！！！！
    'notify_url'         => '默认的订单回调地址',     // 你也可以在下单时单独设置来想覆盖它
    // 'device_info'     => '013467007045764',
    // 'sub_app_id'      => '',
    // 'sub_merchant_id' => '',
    // ...
];

$payment = Factory::payment($options);
```

## 创建订单
- 正常模式
```php
use EasyWeChat\Payment\Order;

$attributes = [
    'trade_type'   => 'JSAPI', // JSAPI，NATIVE，APP...
    'body'         => 'iPad mini 16G 白色',
    'detail'       => 'iPad mini 16G 白色',
    'out_trade_no' => '1217752501201407033233368018',
    'total_fee'    => 5388, // 单位：分
    'notify_url'   => 'http://xxx.com/order-notify', // 支付结果通知网址，如果不设置则会使用配置里的默认地址
    'openid'       => '当前用户的 openid', // trade_type=JSAPI，此参数必传，用户在商户appid下的唯一标识，
    // ...
];

$order = new Order($attributes);
```

- 子服务商模式
```php
use EasyWeChat\Payment\Order;

$attributes = [
    'trade_type'   => 'JSAPI', // JSAPI，NATIVE，APP...
    'body'         => 'iPad mini 16G 白色',
    'detail'       => 'iPad mini 16G 白色',
    'out_trade_no' => '1217752501201407033233368018',
    'total_fee'    => 5388, // 单位：分
    'notify_url'   => 'http://xxx.com/order-notify', // 支付结果通知网址，如果不设置则会使用配置里的默认地址
    'sub_openid'   => '当前用户的 openid', // 如果传入sub_openid, 请在实例化Application时, 同时传入$sub_app_id, $sub_merchant_id
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

> 也许你需要生成二维码图片，参考页面底部相关的包推荐

## 统一下单

[公众号支付](https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=9_1)、[扫码支付](https://pay.weixin.qq.com/wiki/doc/api/native.php?chapter=6_1)、[APP 支付](https://pay.weixin.qq.com/wiki/doc/api/app.php?chapter=9_1) 都统一使用此接口完成订单的创建。

```php
$result = $payment->prepare($order);
if ($result->return_code == 'SUCCESS' && $result->result_code == 'SUCCESS'){
    $prepayId = $result->prepay_id;
}
```

## 支付及退款结果通知

在用户成功支付后，微信服务器会向该 **订单中设置的回调URL** 发起一个 POST 请求，请求的内容为一个 XML。里面包含了所有的详细信息，具体请参考：
[支付结果通用通知](https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=9_7)

而对于用户的退款操作，在退款成功之后也会有一个异步回调通知。

本 SDK 内预置了一个 trait，以方便开发者处理这些通知。

以支付成功结果通知为例，具体用法如下：

1. 在控制器方法中引用这个 trait

```php
use EasyWeChat\Payment\Traits\HandleNotiry;
```

2. 使用相关的处理方法对业务进行处理并向微信服务器返回消息

```php
$response = $this->handleNotify(function($notify, $successful){
    // 你的逻辑
    return true; // 或者错误消息
});

$response->send(); // Laravel 里请使用：return $response;
```

这里需要注意的有几个点：

1. `handleNotify` 只接收一个 [`callable`](http://php.net/manual/zh/language.types.callable.php) 参数，通常用一个匿名函数即可。
2. 该匿名函数接收两个参数，这两个参数分别为：

    - `$notify` 为封装了通知信息的 `EasyWeChat\Support\Collection` 对象，前面已经讲过这里就不赘述了，你可以以对象或者数组形式来读取通知内容，比如：`$notify->total_fee` 或者 `$notify['total_fee']`。
    - `$successful` 这个参数其实就是判断 **用户是否付款成功了**（result_code == 'SUCCESS'）

3. 该函数返回值就是告诉微信 **“我是否处理完成”**，如果你返回一个 `false` 或者一个具体的错误消息，那么微信会在稍后再次继续通知你，直到你明确的告诉它：“我已经处理完成了”，在函数里 `return true;` 代表处理完成。

4. `handleNotify` 返回值 `$response` 是一个 Response 对象，如果你要直接输出，使用 `$response->send()`, 在一些框架里不是输出而是返回：`return $response`。

通常我们的处理逻辑大概是下面这样（**以下只是伪代码**）：

```php
$response = $this->handleNotify(function($notify, $successful){
    // 使用通知里的 "微信支付订单号" 或者 "商户订单号" 去自己的数据库找到订单
    $order = 查询订单($notify->out_trade_no);

    if (!$order) { // 如果订单不存在
        return 'Order not exist.'; // 告诉微信，我已经处理完了，订单没找到，别再通知我了
    }

    // 如果订单存在
    // 检查订单是否已经更新过支付状态
    if ($order->paid_at) { // 假设订单字段“支付时间”不为空代表已经支付
        return true; // 已经支付成功了就不再更新了
    }

    // 用户是否支付成功
    if ($successful) {
        // 不是已经支付状态则修改为已经支付状态
        $order->paid_at = time(); // 更新支付时间为当前时间
        $order->status = 'paid';
    } else { // 用户支付失败
        $order->status = 'paid_fail';
    }

    $order->save(); // 保存订单

    return true; // 返回处理完成
});

return $response;
```

> 注意：请把 “支付成功与否” 与 “是否处理完成” 分开，它俩没有必然关系。
> 比如：微信通知你用户支付完成，但是支付失败了(result_code 为 'FAIL')，你应该**更新你的订单为支付失败**，但是要**告诉微信处理完成**。

处理扫码支付通知的方法名为 `handleScanNotify`，处理退款通知的方法名为 `handleRefundNotify`，请注意区分使用。


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
$payment->reverseByTransactionId($orderNo);
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
$payment->queryByTransactionId($orderNo);
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
$payment->refund(订单号，退款单号，总金额，退款金额，操作员，退款单号类型(out_trade_no/transaction_id)，退款账户(REFUND_SOURCE_UNSETTLED_FUNDS/REFUND_SOURCE_RECHARGE_FUNDS))
```

参考：https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=9_4

例子：

```php
# 1. 使用商户订单号退款
$result = $payment->refund($orderNo, $refundNo, 100); // 总金额 100 退款 100，操作员：商户号
// or
$result = $payment->refund($orderNo, $refundNo, 100, 80); // 总金额 100， 退款 80，操作员：商户号
// or
$result = $payment->refund($orderNo, $refundNo, 100, 80, 1900000109); // 总金额 100， 退款 80，操作员：1900000109
// or
$result = $payment->refund($orderNo, $refundNo, 100, 80, 1900000109, 'out_trade_no'); // 总金额 100， 退款 80，操作员：1900000109, 退款单号：使用商户订单号退款
// or
$result = $payment->refund($orderNo, $refundNo, 100, 80, 1900000109, 'out_trade_no', 'REFUND_SOURCE_RECHARGE_FUNDS'); // 总金额 100， 退款 80，操作员：1900000109, 退款单号：使用商户订单号退款, 退款账户：可用余额退款

# 2. 使用 TransactionId 退款
$result = $payment->refundByTransactionId($transactionId, $refundNo, 100); // 总金额 100 退款 100，操作员：商户号
// or
$result = $payment->refundByTransactionId($transactionId, $refundNo, 100, 80); // 总金额 100， 退款 80，操作员：商户号
// or
$result = $payment->refundByTransactionId($transactionId, $refundNo, 100, 80, 1900000109); // 总金额 100， 退款 80，操作员：1900000109
// or
$result = $payment->refundByTransactionId($transactionId, $refundNo, 100, 80, 1900000109, 'REFUND_SOURCE_RECHARGE_FUNDS'); // 总金额 100， 退款 80，操作员：1900000109，退款账户：可用余额退款
```

> $refundNo 为商户退款单号，自己生成用于自己识别即可。

## 查询退款

提交退款申请后，通过调用该接口查询退款状态。退款有一定延时，用零钱支付的退款20分钟内到账，银行卡支付的退款3个工作日后重新查询退款状态。

```php
$result = $payment->queryRefund($outTradeNo);
// or
$result = $payment->queryRefundByTransactionId($transactionId);
// or
$result = $payment->queryRefundByRefundNo($outRefundNo);
// or
$result = $payment->queryRefundByRefundId($refundId);
```

## 下载对账单

```php
$bill = $payment->downloadBill('20140603')->getContents(); // type: ALL
// or
$bill = $payment->downloadBill('20140603', 'SUCCESS')->getContents(); // type: SUCCESS
// bill 为 csv 格式的内容

// 保存为文件
file_put_contents('YOUR/PATH/TO/bill-20140603.csv', $bill);
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

## 授权码查询OPENID接口

```php
$response = $payment->authCodeToOpenId($authCode);
$response->openid;
```