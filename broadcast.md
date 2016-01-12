title: 群发
---

微信的群发消息接口有各种乱七八糟的注意事项及限制，具体请阅读微信官方文档：http://mp.weixin.qq.com/wiki/15/5380a4e6f02f2ffdc7981a8ed7a40753.html

## 获取实例

```php
<?php
use EasyWeChat\Foundation\Application;
// ...
$app = new Application($options);

$broadcast = $app['broadcast'];

```

## API

### 群发消息给所有粉丝

```php
$broadcast->send($messageType, $message);

// 别名方式
$broadcast->sendText($message);
$broadcast->sendNews($message);
$broadcast->sendVoice($message);
$broadcast->sendImage($message);
$broadcast->sendVideo($message);
$broadcast->sendCard($message);
```

### 群发消息给指定组

```php
$broadcast->send($messageType, $message, $groupId);

// 别名方式
$broadcast->sendText($message, $groupId);
$broadcast->sendNews($message, $groupId);
$broadcast->sendVoice($message, $groupId);
$broadcast->sendImage($message, $groupId);
$broadcast->sendVideo($message, $groupId);
$broadcast->sendCard($message, $groupId);
```

### 群发消息给指定用户

可以是一个用户，也可以是多个用户，但必须是数组。

```php
$broadcast->send($messageType, $message, [$openId1, $openId2]);

// 别名方式
$broadcast->sendText($message, [$openId1, $openId2]);
$broadcast->sendNews($message, [$openId1, $openId2]);
$broadcast->sendVoice($message, [$openId1, $openId2]);
$broadcast->sendImage($message, [$openId1, $openId2]);
$broadcast->sendVideo($message, [$openId1, $openId2]);
$broadcast->sendCard($message, [$openId1, $openId2]);
```

### 发送预览群发消息给指定的 `openId` 用户

```php
$broadcast->preview($messageType, $message, $openId);

// 别名方式
$broadcast->previewText($message, $openId);
$broadcast->previewNews($message, $openId);
$broadcast->previewVoice($message, $openId);
$broadcast->previewImage($message, $openId);
$broadcast->previewVideo($message, $openId);
$broadcast->previewCard($message, $openId);
```

### 发送预览群发消息给指定的微信号用户

```php
$broadcast->previewByName($messageType, $message, $wxname);

// 别名方式
$broadcast->previewTextByName($message, $wxname);
$broadcast->previewNewsByName($message, $wxname);
$broadcast->previewVoiceByName($message, $wxname);
$broadcast->previewImageByName($message, $wxname);
$broadcast->previewVideoByName($message, $wxname);
$broadcast->previewCardByName($message, $wxname);
```

### 删除群发消息

```php
$broadcast->delete($msgId);
```

### 查询群发消息发送状态

```php
$broadcast->status($msgId);
```

上面提到的 `$messageType` 、`$message` 可以是：

- `$messageType = Broadcast::MSG_TYPE_NEWS;` 图文消息类型，所对应的 `$message` 为 media_id
- `$messageType = Broadcast::MSG_TYPE_TEXT;` 文本消息类型，所对应的 `$message` 为一个文本字符串
- `$messageType = Broadcast::MSG_TYPE_VOICE;` 语音消息类型，所对应的 `$message` 为 media_id
- `$messageType = Broadcast::MSG_TYPE_IMAGE;` 图片消息类型，所对应的 `$message` 为 media_id
- `$messageType = Broadcast::MSG_TYPE_CARD;` 卡券消息类型，所对应的 `$message` 为 card_id
- `$messageType = Broadcast::MSG_TYPE_VIDEO;` 视频消息类型，群发视频消息给组或预览群发视频消息给用户时所对应的 `$message` 为
`media_id`，群发视频消息给指定用户时所对应的 `$message` 为一个数组 `['MEDIA_ID', 'TITLE', 'DESCRIPTION']`

有关群发信息的更多细节请参考微信官方文档：http://mp.weixin.qq.com/wiki/15/5380a4e6f02f2ffdc7981a8ed7a40753.html
