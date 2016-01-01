title: 群发
---

微信的群发消息接口有各种乱七八糟的注意事项及限制，具体请阅读微信官方文档：http://mp.weixin.qq.com/wiki/15/5380a4e6f02f2ffdc7981a8ed7a40753.html

本 SDK 由 `Overtrue\Wechat\Broadcast` 提供微信群发相关服务。

### 获取实例

```php
<?php

// ...
$app = new Application($options);

$broadcast = $app['broadcast'];

```

### API

+ `$broadcast->send($messageType, $message);` 群发消息给所有粉丝
+ `$broadcast->send($messageType, $message, $groupId);` 群发消息给指定组
+ `$broadcast->send($messageType, $message, array($openId1, $openId2));` 群发消息给指定用户，可以是一个用户，也可以是多个用户，但必须是数组
+ `$broadcast->preview($messageType, $message, $openId);` 发送预览群发消息给指定的 openid 用户
+ `$broadcast->preview($messageType, $message, $wxname, Broadcast::PREVIEW_BY_WXH);` 发送预览群发消息给指定的微信号用户
+ `$broadcast->delete($msgId);` 删除群发消息
+ `$broadcast->status($msgId);` 查询群发消息发送状态

这里的 `$messageType` 、`$message`可以是：

+ `$messageType = Broadcast::MSG_TYPE_NEWS;` 图文消息类型，所对应的 `$message` 为 media_id
+ `$messageType = Broadcast::MSG_TYPE_TEXT;` 文本消息类型，所对应的 `$message` 为一个文本字符串
+ `$messageType = Broadcast::MSG_TYPE_VOICE;` 语音消息类型，所对应的 `$message` 为 media_id
+ `$messageType = Broadcast::MSG_TYPE_IMAGE;` 图片消息类型，所对应的 `$message` 为 media_id
+ `$messageType = Broadcast::MSG_TYPE_CARD;` 卡券消息类型，所对应的 `$message` 为 card_id
+ `$messageType = Broadcast::MSG_TYPE_VIDEO;` 视频消息类型，群发视频消息给组或预览群发视频消息给用户时所对应的 `$message` 为 media_id，群发视频消息给指定用户时所对应的 `$message` 为一个数组 `array( 'MEDIA_ID', 'TITLE', 'DESCRIPTION')`

有关群发信息的更多细节请参考微信官方文档：http://mp.weixin.qq.com/wiki/15/5380a4e6f02f2ffdc7981a8ed7a40753.html
