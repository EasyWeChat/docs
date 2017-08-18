# 服务端

## 实例化

```php
<?php
use EasyWeChat\Factory;

$config = [
  'app_id'   => '开放平台第三方平台 APPID',
  'secret'   => '开放平台第三方平台 Secret',
  'token'    => '开放平台第三方平台 Token',
  'aes_key'  => '开放平台第三方平台 AES Key'
];

$openPlatform = Factory::openPlatform($config);
```

------

## 推送事件

公众号第三方平台推送的有四个事件：

​	授权成功 `authorized`

​	授权更新 `updateauthorized`

​	授权取消 `unauthorized`

​	VerifyTicket  `component_verify_ticket`

SDK 默认会处理事件 `component_verify_ticket` ，并会缓存 `verify_ticket` 所以如果你暂时不需要处理其他事件，直接这样使用即可：

```php
$server = $openPlatform->server;

return $server->serve();
```



## 自定义消息处理器

> *消息处理器详细说明见公众号开发 - 服务器一节*

```php
use EasyWeChat\OpenPlatform\Server\Guard;

$server = $openPlatform->server;

// 处理授权成功事件
$server->push(function ($message) {
    // ...
}, Guard::EVENT_AUTHORIZED);

// 处理授权更新事件
$server->push(function ($message) {
    // ...
}, Guard::EVENT_UPDATE_AUTHORIZED);

// 处理授权取消事件
$server->push(function ($message) {
    // ...
}, Guard::EVENT_UNAUTHORIZED);
```

