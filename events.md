title: 事件
---

所有的事件都可以很方便的监听与处理，与监听消息一样，同样支持监听全部类型或者指定类型。


在本 SDK 中，服务端的操作，例如：服务器认证，监听事件，接收用户消息等所有微信向我们自有服务器发起请求的，都交由 `Overtrue\Wechat\Server` 来处理。

### 获取服务端实例

```php
<?php
use Overtrue\Wechat\Server;

$appId          = 'wx3cf0f39249eb0e60';
$token          = 'hellotest';
$encodingAESKey = 'EJThPazwzO4k1cyXJnwQtL60zBdhWvFaHb4emv0dLVN';

$server = new Server($appId, $token, $encodingAESKey);
```

### 监听事件：

#### 语法：

```php
$server->on('event',  callable $callback);
// or
$server->on('event',  string $eventType, callable $callback);
```

#### 参数说明：

- `$eventType` string, 指定要处理的消息类型，ex：`image`
- `$callback` callable, 回调函数，Closure 匿名函数，或者一切可调用的方法或者函数

`$callback` 接收一个参数 `$event` ， `$event` 包含以下基本属性：

    ToUserName   接收者 ID（公众号 ID）
    FromUserName 发送方帐号（一个 OpenID）
    CreateTime   消息创建时间 （整型）
    MsgType      event
    Event        事件类型，ex: subscribe
    EventKey     事件 Key 值，与自定义菜单接口中 Key 值对应

完整举例:

```php
<?php

use Overtrue\Wechat\Server;
use Overtrue\Wechat\Message;

$appId          = 'wx3cf0f39249eb0e60';
$secret         = 'f1c242f4f28f735d4687abb469072a29';
$token          = 'hellotest';
$encodingAESKey = 'EJThPazwzO4k1cyXJnwQtL60zBdhWvFaHb4emv0dLVN';

$server = new Server($appId, $token, $encodingAESKey);

// 监听所有事件
$server->on('event', function($event) {
    error_log('收到取消关注事件，取消关注者openid: ' . $event['FromUserName']);
});

// 只监听指定类型事件
$server->on('event', 'subscribe', function($event) {

    error_log('收到关注事件，关注者openid: ' . $event['FromUserName']);

    return Message::make('text')->content('感谢您关注');
});

$res = $server->serve();
echo $res; // or return $res; 在某些框架中需要返回字符串
```

关于事件类型请参考微信官方文档：http://mp.weixin.qq.com/wiki/2/5baf56ce4947d35003b86a9805634b1e.html