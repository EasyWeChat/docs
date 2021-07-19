# 自定义版交易组件及开放接口 - 接入商品前必需接口 

## 获取类目详情

API:

```
$app->shop_basic->getCat();
```

> 注意：该接口拉到的是【全量】三级类目数据，数据回包大小约为2MB。所以请商家自己做好缓存，不要频繁调用（有严格的频率限制），该类目数据不会频率变动，推荐商家每天调用一次更新商家自身缓存


## 上传图片

API:

```
// 图片的绝对路径 图片大小限制2MB
$app->shop_basic->imgUpload($imageFilePath);
```

> 此接口目前只用于品牌申请和类目申请。


## 上传品牌信息

API:

```
// 详细内容参考微信官方文档
$brand = [
    'license' => 'https://img.zhls.qq.com/3/609b98f7e0ff43d59ce6d9cca636c3e0.jpg',
    'brand_info' => [
        'brand_audit_type' => 1,
        'trademark_type' => '29',
        .....
    ],

];

$app->shop_basic->auditBrand($brand);
```

> 使用到的图片的地方，可以使用url或media_id


## 上传类目资质

API:

```
// 详细内容参考微信官方文档
$category = [
    'license' => 'https://img.zhls.qq.com/3/609b98f7e0ff43d59ce6d9cca636c3e0.jpg',
    'category_info' => [
        'level1' => 7719,
        'level2' => 7439,
        'level3' => 7448,
        'certificate' => [
            'https://img.zhls.qq.com/3/609b98f7e0ff43d59ce6d9cca636c3e0.jpg'
        ]
    ],

];

$app->shop_basic->auditCategory($category);
```

> 使用到的图片的地方，可以使用url或media_id


## 获取审核结果

API:

```
// 根据审核id，查询品牌和类目的审核结果。
$app->shop_basic->auditResult($auditId);
```


## 获取小程序资质

API:

```
$app->shop_basic->getMiniAppCertificate($reqType);
```
