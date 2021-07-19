# 自定义版交易组件及开放接口 - 申请接入接口 

## 接入申请

API:

```
$app->shop_register->apply();
```

> 通过此接口开通自定义版交易组件，将同步返回接入结果，不再有异步事件回调。


## 获取接入状态

API:

```
$app->shop_register->check();
```

> 如果账户未接入，将返回错误码1040003


## 完成接入任务

API:

```
$app->shop_register->finishAccessInfo($accessInfoItem);
```

> $accessInfoItem 变量含义：6:完成spu接口，7:完成订单接口，8:完成物流接口，9:完成售后接口，10:测试完成，11:发版完成


## 获取接入状态

API:

```
$app->shop_register->applyScene();
```

> 目前 $sceneGroupId 只支持 1:视频号、公众号场景，所以可以不传。