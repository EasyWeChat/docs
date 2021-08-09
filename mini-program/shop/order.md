# 自定义版交易组件及开放接口 - 订单接口


## 生成订单

API:

```
$order = [
    'create_time' => '2020-03-25 13:05:25'
    'type' => 0,
    ......
];

$app->shop_order->add($productId);
```

> 注意：该接口可重入，如果order_id或out_order_id已存在，会直接更新整个订单数据


## 同步订单支付结果

API:

```
$pay = [
    'out_order_id' => 'abc123'
    'action_type' => 1,
    ......
];

$app->shop_order->pay($productId);
```


## 获取订单

API:

```
// 商家自定义订单ID 微信侧订单id （二选一）
$orderId = [
    'out_order_id' => 'abc123'
    'order_id' => 122221,
];

$openid = 'abcdefg12112';

$app->shop_order->get($openid, $orderId);
```


## 检查场景值是否在支付校验范围内

API:

```
$app->shop_order->sceneCheck(1175);
```
