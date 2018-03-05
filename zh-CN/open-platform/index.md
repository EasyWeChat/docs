# 微信开放平台第三方平台

## 实例化

```php
<?php
use EasyWeChat\Factory;

$config = [
  'app_id'   => '开放平台第三方平台 APPID',
  'secret'   => '开放平台第三方平台 Secret',
  开放平台并没有aeskey啊？还有这个token
  'token'    => '开放平台第三方平台 Token',
  'aes_key'  => '开放平台第三方平台 AES Key'
];

$openPlatform = Factory::openPlatform($config);
```

## 获取用户授权页 URL

```php
$openPlatform->getPreAuthorizationUrl('https://easywechat.com/callback'); // 传入回调URI即可
```

## 使用授权码换取接口调用凭据和授权信息

在用户在授权页授权流程完成后，授权页会自动跳转进入回调URI，并在URL参数中返回授权码和过期时间，如：(https://easywechat.com/callback?auth_code=xxx&expires_in=600)

```php
$openPlatform->handleAuthorize(string $authCode = null);
```

> $authCode 不传的时候会获取 url 中的 auth_code 参数值

## 获取授权方的帐号基本信息

```php
$openPlatform->getAuthorizer(string $appId);
```

## 获取授权方的选项设置信息

```php
$openPlatform->getAuthorizerOption(string $appId, string $name);
```

## 设置授权方的选项信息

```php
$openPlatform->setAuthorizerOption(string $appId, string $name, string $value);
```

## 获取已授权的授权方列表

```php
$openPlatform->getAuthorizers(int $offset = 0, int $count = 500)
```
