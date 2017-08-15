# 群发

微信的群发消息接口有各种乱七八糟的注意事项及限制，具体请阅读微信官方文档。

## 获取实例

```php
$broadcast = $app->broadcasting;
```

## 发送消息

以下所有方法均有第二个参数 `$to` 用于指定接收人，默认为全部用户。

### 文本消息

```php
$broadcast->sendText("大家好！欢迎使用 EasyWeChat。");
// 同时指定目标用户
// 至少两个用户的 openid，必须是数组。
$broadcast->sendText("大家好！欢迎使用 EasyWeChat。", [$openid1, $openid2]);
```

### 图文消息

```php
$broadcast->sendNews($mediaId);
```

### 图片消息

```php
$broadcast->sendImage($mediaId);
```

### 语音消息

```php
$broadcast->sendVoice($mediaId);
```

### 视频消息

用于群发的视频消息，需要先创建消息对象，

```php
// 1. 先上传视频素材用于群发：
$video = '/path/to/video.mp4';
$videoMedia = $app->media->uploadVideoForBroadcasting($video, '视频标题', '视频描述');

// 结果如下：
//{
//  "type":"video",
//  "media_id":"IhdaAQXuvJtGzwwc0abfXnzeezfO0NgPK6AQYShD8RQYMTtfzbLdBIQkQziv2XJc",
//  "created_at":1398848981
//}

// 2. 使用上面得到的 media_id 群发视频消息
$broadcast->sendVideo($videoMedia['media_id']);
```

### 卡券消息

```php
$broadcast->sendCard($cardId);
```

### 群发消息给指定组

```php
$broadcast->send($messageType, $message, $groupId);

// 别名方式
$broadcast->sendText($text, $groupId);
$broadcast->sendNews($mediaId, $groupId);
$broadcast->sendVoice($mediaId, $groupId);
$broadcast->sendImage($mediaId, $groupId);
$broadcast->sendVideo($message, $groupId);
$broadcast->sendCard($cardId, $groupId);
```

### 群发消息给指定用户

至少两个用户的 openid，必须是数组。

```php
$broadcast->sendText($text, [$openId1, $openId2]);
$broadcast->sendNews($mediaId, [$openId1, $openId2]);
$broadcast->sendVoice($mediaId, [$openId1, $openId2]);
$broadcast->sendImage($mediaId, [$openId1, $openId2]);
$broadcast->sendVideo($message, [$openId1, $openId2]);
$broadcast->sendCard($cardId, [$openId1, $openId2]);
```

### 发送预览群发消息给指定的 `openId` 用户

```php
$broadcast->previewText($text, $openId);
$broadcast->previewNews($mediaId, $openId);
$broadcast->previewVoice($mediaId, $openId);
$broadcast->previewImage($mediaId, $openId);
$broadcast->previewVideo($message, $openId);
$broadcast->previewCard($cardId, $openId);
```

### 发送预览群发消息给指定的微信号用户

```php
$broadcast->previewTextByName($text, $wxname);
$broadcast->previewNewsByName($mediaId, $wxname);
$broadcast->previewVoiceByName($mediaId, $wxname);
$broadcast->previewImageByName($mediaId, $wxname);
$broadcast->previewVideoByName($message, $wxname);
$broadcast->previewCardByName($cardId, $wxname);
```

### 删除群发消息

```php
$broadcast->delete($msgId);
```

### 查询群发消息发送状态

```php
$broadcast->status($msgId);
```