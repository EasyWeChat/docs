title: 微信开放平台
---

## 实例化

```php
<?php
use EasyWeChat\Foundation\Application;

$options = [
    // ...
    'open_platform' => [
        'app_id'   => 'component-app-id',
        'secret'   => 'component-app-secret',
        'token'    => 'component-token',
        'aes_key'  => 'component-aes-key'
        ],
    // ...
    ];

$app = new Application($options);
$openPlatform = $app->open_platform;
```

## 微信服务器推送事件

公众号第三方平台推送的有四个事件：

+ 授权成功（`authorized`）
+ 授权更新（`updateauthorized`）
+ 授权取消（`unauthorized`）
+ 推送 ComponentVerifyTicket（`component_verify_ticket`）

> 在公众号第三方平台创建审核通过后，微信服务器会向其“授权事件接收URL”每隔 10 分钟推送一次 `component_verify_ticket`。

> SDK 内部已实现缓存 `component_veirfy_ticket`，开发者无需另行处理该事件。
> 其余事件需要开发者自行处理。

* 注：需要在URL路由中写上触发代码，并且注册路由后需要等待微信服务器推送 `component_verify_ticket`，才有权限进行其他操作，否则报"Component verify ticket does not exists."

Example:

```php
use EasyWeChat\OpenPlatform\Guard;

$server = $openPlatform->server;

$server->setMessageHandler(function($event) use ($openPlatform) {
    // 事件类型常量定义在 \EasyWeChat\OpenPlatform\Guard 类里
    switch ($event->InfoType) {
        case Guard::EVENT_AUTHORIZED: // 授权成功
            $authorizationInfo = $openPlatform->getAuthorizationInfo($event->AuthorizationCode);
            // 保存数据库操作等...
        case Guard::EVENT_UNAUTHORIZED: // 更新授权
            // 更新数据库操作等...
        case Guard::EVENT_UPDATE_AUTHORIZED: // 授权取消
            // 更新数据库操作等...
    }
});

$response = $server->serve();

$response->send(); // Laravel 里请使用：return $response;
```

## 预授权

### 获取预授权 Code

Example:

```php
$openPlatform->pre_auth->getCode();
```

### 获取预授权 URL

Example:

```php
// 直接跳转
$response = $openPlatform->pre_auth->redirect('https://your-domain.com/callback');

// 获取跳转的 URL
$response->getTargetUrl();

```
用户授权后会带上 `auth_code` 跳转到 `https://your-domain.com/callback?auth_code=xxxxxxx`

## API 列表

### 使用授权码换取公众号的接口调用凭据和授权信息

```php
// 使用授权码换取公众号的接口调用凭据和授权信息
// Optional: $authorizationCode 不传值时会自动获取 URL 中 auth_code 值
$openPlatform->getAuthorizationInfo($authorizationCode = null);
```

### 获取授权方的公众号帐号基本信息

```php
$openPlatform->getAuthorizerInfo($authorizerAppId);
```

### 获取授权方的选项设置信息

```php
$openPlatform->getAuthorizerOption($authorizerAppId, $optionName);
```

### 设置授权方的选项信息

```php
$openPlatform->setAuthorizerOption($authorizerAppId, $optionName, $optionValue);
```

## 调用授权方 API

通过该方法会获得一个 `\EasyWeChat\Foundation\Application` 实例。
> 当调用授权方 API 后，SDK 内部会自动获取和刷新 `AuthorizerAccessToken` 有效期。
> 所以开发者无需处理授权方公众号的接口调用凭据 `AuthorizerAccessToken`。

```php
// 传递 AuthorizerAppId 和 AuthorizerRefreshToken（注意不是 AuthorizerAccessToken）即可。
$app = $openPlatform->createAuthorizer($authorizerAppId, $authorizerRefreshToken);
// 调用方式与普通调用一致。
```
