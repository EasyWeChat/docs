title: 素材管理
---

在微信里的图片，音乐，视频等等都需要先上传到微信服务器作为素材才可以在消息中使用。

> 请注意：

>     1. 限制：
>       - 图片（image）: 1M，支持 bmp/png/jpeg/jpg/gif 格式
>       - 语音（voice）：2M，播放长度不超过 60s，支持 mp3/wma/wav/amr 格式
>       - 视频（video）：10MB，支持MP4格式
>       - 缩略图（thumb）：64KB，支持JPG格式

>     2. `media_id` 是可复用的；

>     3. 素材分为 `临时素材` 与 `永久素材`， 临时素材媒体文件在后台保存时间为3天，即 3 天后 `media_id` 失效；

>     4. 新增的永久素材也可以在公众平台官网素材管理模块中看到；

>     5. 永久素材的数量是有上限的，请谨慎新增。图文消息素材和图片素材的上限为5000，其他类型为1000；

本 SDK 中上传素材通过 `Overtrue\Wechat\Media` 提供素材管理服务。

### 获取实例

```php
<?php

use Overtrue\Wechat\Media;

$appId  = 'wx3cf0f39249eb0e60';
$secret = 'f1c242f4f28f735d4687abb469072a29';

$media = new Media($appId, $secret);
```

### API列表：

- `$media->image($path);`, 上传临时图片；
- `$media->voice($path);`, 上传临时声音；
- `$media->video($path, $title, $description);`, 上传临时视频；
- `$media->thumb($path);`, 上传临时缩略图，用于视频封面或者音乐封面；
- `$media->news(array $articles);`, 上传永久图文消息，图文没有临时；
- `$media->updateNews($mediaId, array $article, $index);`, 修改永久图文消息，要更新的文章在图文消息中的位置（多图文消息时，此字段才有意义，单图片忽略此参数），第一篇为0；
- `$media->forever()->image($path);` 上传永久图片；
- `$media->forever()->voice($path);` 上传永久声音；
- `$media->forever()->video($path);` 上传永久视频；
- `$media->forever()->thumb($path);` 上传永久缩略图；
- `$media->lists($type, $offset, $count);` 获取永久素材列表，参考：[微信公众平台开发者文档：获取永久素材列表](http://mp.weixin.qq.com/wiki/12/2108cd7aafff7f388f41f37efa710204.html)
- `$media->stats($type = null);` 获取素材计数，不指定 `$type` 则返回全部
- `$media->delete($mediaId);` 删除永久素材；
- `$media->download($mediaId, $filename);` 下载临时素材到本地，`$filename` 为目标路径带文件名，例如：`$media->download($mediaId, __DIR__ . '/test.jpg');`；
- `$media->forever()->download($mediaId, $filename);` 下载永久素材到本地；

> 上传永久图文消息 `$articles` 结构为：

    [{
       "title": TITLE,
       "thumb_media_id": THUMB_MEDIA_ID,
       "author": AUTHOR,
       "digest": DIGEST,
       "show_cover_pic": SHOW_COVER_PIC(0 / 1),
       "content": CONTENT,
       "content_source_url": CONTENT_SOURCE_URL
     },
     //若新增的是多图文素材，则此处应还有几段articles结构
    ]

字段说明：

    - title: 标题
    - thumb_media_id: 图文消息的封面图片素材id（必须是永久mediaID）
    - author: 作者
    - digest: 图文消息的摘要，仅有单图文消息才有摘要，多图文此处为空
    - show_cover_pic: 是否显示封面，0为false，即不显示，1为true，即显示
    - content: 图文消息的具体内容，支持HTML标签，必须少于2万字符，小于1M，且此处会去除JS
    - content_source_url: 图文消息的原文地址，即点击“阅读原文”后的URL

### 示例：

创建图片消息：

```php
<?php
use Overtrue\Wechat\Media;

$appId  = 'wx3cf0f39249eb0e60';
$secret = 'f1c242f4f28f735d4687abb469072a29';

$media   = new Media($appId, $secret);
$imageId = $media->image(__DIR__ . '/demo.jpg'); // 上传并返回媒体ID

$message = Message::make('image')->media($imageId);
```

更多请参考官方文档：http://mp.weixin.qq.com/wiki/home/index.html `素材管理` 章节