# 自定义版交易组件及开放接口 - 售后接口


## 创建售后

API:

```
$aftersale = [
    'out_order_id' => 'abcdesa121'
    'out_aftersale_id' => 'af201121233',
    ......
];

$app->shop_aftersale->add($aftersale);
```

> 注意：订单原始状态为10, 200, 250时会返回错误码100000


## 获取售后

API:

```
$order = [
    'out_order_id' => 'abcdesa121'
    'openid' => 'af201121233',
];

$app->shop_aftersale->get($order);
```


## 更新售后

API:

```
$order = [
    'out_order_id' => 'abcdesa121'
    'openid' => 'af201121233',
];

$aftersale = [
    'status' => 1
    'finish_all_aftersale' => 0,
    'out_aftersale_id' => 'abcdefa123',
];

$app->shop_aftersale->update($order, $aftersale);
```

