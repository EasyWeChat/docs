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
        'token'  => 'component-token',
        'aes_key'    => 'component-aes-key'
        ],
    // ..
    ];

$app = new Application($options);
$open_platform = $app->open_platform;
```

### 监听微信服务器推送事件

```php

$open_platform->server->listen(function ($event) {
    switch ($event->InfoType) {
        case 'authorized':          // 授权成功
            // logic code here...
        case 'unauthorized':        // 取消授权
            // logic code here...
        case 'updateauthorized':    // 授权更新
            // logic code here...
        }
    });
```


### 推送component_verify_ticket协议

在公众号第三方平台创建审核通过后，微信服务器会向其“授权事件接收URL”每隔10分钟定时推送component_verify_ticket。SDK内部已实现缓存component_veirfy_ticket，无需开发者另行缓存。

注：需要在URL路由中写上触发代码，并且注册路由后需要等待微信服务器推送verify_ticket，才能进行后续操作，否则报"Component verify ticket does not exists."

```php
// Example
public function serve(){
    return $open_platform->server->listen();
}

```


### 获取预授权网址
```php
$open_platform->pre_auth
    ->setRedirectUri('http://domain.com/callback')
    ->getAuthLink();

```

用户授权后会带上code跳转到$redirect_uri

###### 使用授权码换取公众号的接口调用凭据和授权信息
```php
$authorizer = $open_platform->authorizer;

// 使用授权码换取公众号的接口调用凭据和授权信息
$authorizer->getAuthInfo($authorization_code);
```


##### 获取授权方的公众号帐号基本信息
```php
$authorizer = $open_platform->authorizer;

$authorizer->getAuthorizerInfo($authorizer_appid);

```

##### 获取授权方的选项设置信息
```php
$authorizer = $open_platform->authorizer;

$authorizer->getAuthorizerOption($authorizer_appid, $option_name);

```

##### 设置授权方的选项信息
```php
$authorizer = $open_platform->authorizer;

$authorizer->setAuthorizerOption($authorizer_appid, $option_name, $option_value);

```