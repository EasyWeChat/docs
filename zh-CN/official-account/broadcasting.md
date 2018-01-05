# 群发

微信的群发消息接口有各种乱七八糟的注意事项及限制，具体请阅读微信官方文档。

## 获取实例

```php
$broadcast = $app->broadcasting;
```

## 发送消息

以下所有方法均有第二个参数 `$to` 用于指定接收对象：

- 当 `$to` 为整型时为标签 id
- 当 `$to` 为数组时为用户的 openid 列表（至少两个用户的 openid）
- 为 `$to` 为 `null` 时表示全部用户

```php
$broadcast->sendMessage(Message $message, array | int $to = null);
```

下面的别名方法 `sendXXX` 都是基于上面 `sendMessage` 方法的封装。

### 文本消息

```php
$broadcast->sendText("大家好！欢迎使用 EasyWeChat。");

// 指定目标用户
// 至少两个用户的 openid，必须是数组。
$broadcast->sendText("大家好！欢迎使用 EasyWeChat。", [$openid1, $openid2]);

// 指定标签组用户
$broadcast->sendText("大家好！欢迎使用 EasyWeChat。", $tagId); // $tagId 必须是整型数字
```

### 图文消息

```php
$broadcast->sendNews($mediaId);
$broadcast->sendNews($mediaId, [$openid1, $openid2]);
$broadcast->sendNews($mediaId, $tagId);
```

### 图片消息

```php
$broadcast->sendImage($mediaId);
$broadcast->sendImage($mediaId, [$openid1, $openid2]);
$broadcast->sendImage($mediaId, $tagId);
```

### 语音消息

```php
$broadcast->sendVoice($mediaId);
$broadcast->sendVoice($mediaId, [$openid1, $openid2]);
$broadcast->sendVoice($mediaId, $tagId);
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
$broadcast->sendCard($mediaId, [$openid1, $openid2]);
$broadcast->sendCard($mediaId, $tagId);
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

> $wxanme 是用户的微信号，比如：notovertrue

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