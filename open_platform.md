title: 微信开放平台
---

### 实例化

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
$open_platform = $app->open_platform;
```

### 监听微信服务器推送事件

公众号第三方平台推送的有四个事件：授权成功(`authorized`)，授权更新(`updateauthorized`)，授权取消（`unauthorized`），以及 `component_verify_ticket`。

本 SDK 默认处理方式为：

- `authorized` / `updateauthorized`: 获取授权方(Authorizer)的所有信息，并缓存 `authorizer_access_token` 和 `authorizer_refresh_token`，授权方的信息则需要开发者手动处理。
- `unauthorized`: 删除 `authorizer_access_token` 和 `authorizer_refresh_token` 的缓存。
- `component_verify_ticket`: 缓存 `component_veirfy_ticket`。

当然也允许自定义处理这些事件，不过以上默认处理仍然会先执行，为的是帮助开发者免去缓存的困扰。

```php
// 默认处理方式
$open_platform->server->serve();

// 自定义处理
$open_platform->server->setMessageHandler(function($event) {
    // 事件类型常量定义在 \EasyWeChat\OpenPlatform\Guard 类里
    switch ($event->InfoType) {
        case 'authorized':
            // ...
        case 'unauthorized':
            // ...
        case 'updateauthorized':
            // ...
        case 'component_verify_ticket':
            // ...
    }
});
$open_platform->server->serve();

// 或者
$open_platform->server->listen(function ($event) {
    switch ($event->InfoType) {
        // ...
    }
});
```

#### 授权成功，授权更新

这两个事件下，SDK 默认抓取了所有授权方所有的信息，并缓存 `authorizer_access_token` 和 `authorizer_refresh_token`，授权方的信息为原微信 API 的返回结果，由开发者自行处理，比如保存到数据库。

```php
// 自定义处理
// 其中 $event 变量里有微信推送事件本身的信息，也有授权方所有的信息。
$open_platform->server->setMessageHandler(function($event) {
    // 事件类型常量定义在 \EasyWeChat\OpenPlatform\Guard 类里
    switch ($event->InfoType) {
        case 'authorized':
            // 授权信息，主要是 token 和授权域
            $info1 = $event->authorization_info;
            // 授权方信息，就是授权方公众号的信息了
            $info2 = $event->authorizer_info;
    }
});
```

目前 SDK 对这两个事件的处理方式没有区别。

#### 授权取消

SDK 默认处理：删除 `authorizer_access_token` 和 `authorizer_refresh_token` 的缓存。开发者可以自行处理数据库删除授权方信息等操作。

#### 推送 component_verify_ticket

在公众号第三方平台创建审核通过后，微信服务器会向其“授权事件接收URL”每隔10分钟定时推送 `component_verify_ticket`。SDK 内部已实现缓存 `component_veirfy_ticket`，无需开发者另行缓存。

注：需要在URL路由中写上触发代码，并且注册路由后需要等待微信服务器推送 `component_verify_ticket`，才能进行后续操作，否则报"Component verify ticket does not exists."

### 调用 API

#### 替换 Access Token

第三方必须使用 `authorizer_access_token` 而不是原来的 `access_token` 来调用 API。

替换原来的 access token:

```php
$app = new Application($options);
$app->access_token = $app->open_platform->authorizer_token;
```

#### 设置授权方的 App Id

开发者必须设置授权方来调用 API。

```php
$app = new Application($options);

// 加载授权方信息，比如 $authorizer = Authorizer::find($id);
$authorizerAppId = $authorizer->app_id;

$app->open_platform->authorization->setAuthorizerAppId($authorizerAppId);
```

### 授权 API

#### 获取预授权网址

```php
$open_platform->pre_auth
    ->setRedirectUri('http://domain.com/callback')
    ->getAuthLink();

```

用户授权后会带上 `code` 跳转到 `$redirect_uri`。

#### 使用授权码换取公众号的接口调用凭据和授权信息

```php
$authorizer = $open_platform->authorizer;

// 使用授权码换取公众号的接口调用凭据和授权信息
// Optional: $authorization_code 不传值时会自动获取 URL 中 auth_code 值
$authorizer->getAuthorizationInfo($authorization_code = null);
```

#### 获取授权方的公众号帐号基本信息

```php
$authorizer = $open_platform->authorizer;

$authorizer->getAuthorizerInfo($authorizer_appid);
```

#### 获取授权方的选项设置信息

```php
$authorizer = $open_platform->authorizer;

$authorizer->getAuthorizerOption($authorizer_appid, $option_name);
```

#### 设置授权方的选项信息

```php
$authorizer = $open_platform->authorizer;

$authorizer->setAuthorizerOption($authorizer_appid, $option_name, $option_value);
```
