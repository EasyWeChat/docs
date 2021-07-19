# 自定义版交易组件及开放接口 - 物流接口


## 获取快递公司列表

API:

```
$app->shop_delivery->getCompanyList();
```


## 订单发货

API:

```
// 商家自定义订单ID 微信侧订单id （二选一）
$order = [
    'out_order_id' => 'abc123'
    'order_id' => 122221,
    ......
];
$app->shop_delivery->send($order);
```

> delivery_id请参考获取快递公司列表接口的数据结果，不能自定义，如果对应不上，请传"delivery_id":"OTHERS"。


## 订单确认收货

API:

```
// 商家自定义订单ID 微信侧订单id （二选一）
$order = [
    'out_order_id' => 'abc123'
    'order_id' => 122221,
    'openid' => 'abcdefg11212',
];

$app->shop_delivery->recieve();
```

> 把订单状态从30（待收货）流转到100（完成）