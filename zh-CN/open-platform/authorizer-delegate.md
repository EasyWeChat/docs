# 代授权方实现业务

> 授权方已经把公众号、小程序授权给你的开放平台第三方平台了，接下来的代授权方实现业务只需一行代码即可获得授权方实例。

## 实例化

```php
use EasyWeChat\Factory;

$config = [
    // ...
];

$openPlatform = Factory::openPlatform($config);
```

### 获取授权方实例

```php
// 代公众号实现业务
$officialAccount = $openPlatform->officialAccount(string $appId, string $refreshToken);
// 代小程序实现业务
$miniProgram = $openPlatform->miniProgram(string appId, string $refreshToken);
```

> $appId 为授权方公众号 APPID，非开放平台第三方平台 APPID
>
> $refreshToken 为授权方的 refresh_token，可通过 [获取授权方授权信息](#) 接口获得。



接下来的 API 调用等操作和公众号、小程序的开发一致，请移步到[公众号](#)或[小程序](#)开发章节继续进行开发吧。