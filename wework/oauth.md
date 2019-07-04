# OAuth

> {warning} 此文档为企业微信内部应用开发的网页授权

创建实例：

```php
$config = [
    'corp_id' => 'xxxxxxxxxxxxxxxxx',
    'secret'   => 'xxxxxxxxxx', // 应用的 secret
    'agent_id' => 100001,
];

$app = Factory::work($config);
```

## 获取跳转 URL

```php
$url = $app->oauth->redirect($callbackUrl); // $callbackUrl 为授权回调地址
```

## 获取授权用户信息

在回调页面中，你可以使用以下方式获取授权者信息：

```php
$user = $app->oauth->user();

// 获取用户信息
$user->getId(); // 对应企业微信英文名（userid）
$user->getNickname(); // 对应企业微信名称（name）
$user->getName(); // 对应企业微信 avatar
$user->getEmail(); // 对应企业微信 email
$user->getAvatar(); // 对应企业微信 avatar
$user->getOriginal(); // 获取企业微信接口返回的原始信息
```
