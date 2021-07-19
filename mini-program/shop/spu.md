# 自定义版交易组件及开放接口 - SPU接口 

## 添加商品

API:

```
$product = [
    'out_product_id' => '123456',
    'title' => '任天堂 Nintendo Switch 国行续航增强',
    ......
];

$app->shop_spu->add($product);
```

> 因该接口参数较多，参数之间也有一定的逻辑关系，请仔细参照官方文档进行设置。


## 删除商品

API:

```
// product_id与out_product_id二选一
$productId = [
    'product_id' => 1212，
    'out_product_id' => 'abcde123',
];

$app->shop_register->del($productId);
```

> 从初始值/上架/若干下架状态转换成逻辑删除（删除后不可恢复）


## 撤回商品审核

API:

```
// product_id与out_product_id二选一
$productId = [
    'product_id' => 1212，
    'out_product_id' => 'abcde123',
];

$app->shop_register->delAudit($productId);
```

> 对于审核中（edit_status=2）的商品无法重复提交，需要调用此接口，使商品流转进入未审核的状态（edit_status=1）,即可重新提交商品。


## 获取商品

API:

```
// product_id与out_product_id二选一
$productId = [
    'product_id' => 1212，
    'out_product_id' => 'abcde123',
];

$app->shop_register->get($productId);
```


## 获取商品列表

API:

```
$product = [
    'status' => 5
    'start_create_time' => '2020-12-25 00:00:00',
    'end_create_time' => '2020-12-26 00:00:00'
];

$page = [
    'page' => 1,
    'page_size' => 15,
];

$app->shop_register->getList($product, $page);
```

> 时间范围 create_time 和 update_time 同时存在时，以 create_time 的范围为准


## 更新商品

API:

```
$product = [
    'out_product_id' => '123456',
    'title' => '任天堂 Nintendo Switch 国行续航增强',
    ......
];

$app->shop_register->update($product);
```

> 因该接口参数较多，参数之间也有一定的逻辑关系，请仔细参照官方文档进行设置。


## 免审更新商品字段

API:

```
$product = [
    'out_product_id' => '123456',
    'title' => '任天堂 Nintendo Switch 国行续航增强',
    ......
];

$app->shop_register->updateWithoutAudit($product);
```

> 因该接口参数较多，参数之间也有一定的逻辑关系，请仔细参照官方文档进行设置。


## 上架商品

API:

```
// product_id与out_product_id二选一
$productId = [
    'product_id' => 1212，
    'out_product_id' => 'abcde123',
];

$app->shop_register->listing($productId);
```


## 下架商品

API:

```
// product_id与out_product_id二选一
$productId = [
    'product_id' => 1212，
    'out_product_id' => 'abcde123',
];

$app->shop_register->delisting($productId);
```