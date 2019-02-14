# 物流助手 电子面单

## 获取支持的快递公司列表

```php

$app->express->getAllExpress();

{
  "count": 8,
  "data": [
    {
      "delivery_id": "BEST",
      "delivery_name": "百世快递"
    },
    ...
  ]
}

```

## 生成运单

```

$data 格式 [参考](https://developers.weixin.qq.com/miniprogram/dev/api/addOrder.html)

$app->express->addOrder($data);


 成功返回

{
  "order_id": "01234567890123456789",
  "waybill_id": "123456789",
  "waybill_data": [
    {
      "key": "SF_bagAddr",
      "value": "广州"
    },
    {
      "key": "SF_mark",
      "value": "101- 07-03 509"
    }
  ]
}

失败返回

{
  "errcode": 9300501,
  "errmsg": "delivery logic fail",
  "delivery_resultcode": 10002,
  "delivery_resultmsg": "客户密码不正确"
}

```

## 取消运单

```

$data 格式 [参考](https://developers.weixin.qq.com/miniprogram/dev/api/cancelOrder.html)

$app->express->cancelOrder($data);

```

## 获取运单数据

```

$data 格式 [参考](https://developers.weixin.qq.com/miniprogram/dev/api/getOrder.html)

$app->express->getOrder($data);

```

## 查询运单轨迹

```

$data 格式 [参考](https://developers.weixin.qq.com/miniprogram/dev/api/getOrder.html)

$app->express->getPath($data);

```


