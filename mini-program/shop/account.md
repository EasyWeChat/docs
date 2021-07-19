# 自定义版交易组件及开放接口 - 商家入驻接口


## 获取类目列表

API:

```
$app->shop_account->getCategoryList();
```

## 获取品牌列表

API:

```
$app->shop_account->getBrandList();
```

## 更新商家信息

API:

```
$app->shop_account->updateInfo($path, $phone);
```

> 客服地址和客服联系方式二选一


## 获取商家信息

API:

```
$app->shop_account->getInfo();
```