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

### 获取实例

```php
<?php

$app = new Application($options);

// 永久素材
$material = $app['material'];
// 临时素材
$temporary = $app['material.temporary'];
```

### 永久素材 API：

- `$material->uploadImage($path);`, 上传图片；
- `$material->uploadVoice($path);`, 上传声音；
- `$material->uploadVideo($path, $title, $description);`, 上传视频；
- `$material->uploadThumb($path);`, 上传缩略图，用于视频封面或者音乐封面；
- `$material->uploadArticle(array $articles);`, 上传永久图文消息，图文没有临时；
- `$material->updateArticle($mediaId, array $article, $index);`, 修改永久图文消息，要更新的文章在图文消息中的位置（多图文消息时，此字段才有意义，单图片忽略此参数），第一篇为0；
- `$material->uploadArticleImage($path);` 上传永久文章内容图片；
- `$material->get($mediaId);` 获取永久素材
- `$media->lists($type, $offset, $count);` 获取永久素材列表，参考：[微信公众平台开发者文档：获取永久素材列表](http://mp.weixin.qq.com/wiki/12/2108cd7aafff7f388f41f37efa710204.html)
- `$media->stats($type);` 获取素材计数，不指定 `$type` 则返回全部
- `$media->delete($mediaId);` 删除永久素材；

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


#### 临时素材 API

- `$temporary->uploadImage($path);`, 上传图片；
- `$temporary->uploadVoice($path);`, 上传声音；
- `$temporary->uploadVideo($path, $title, $description);`, 上传视频；
- `$temporary->uploadThumb($path);`, 上传缩略图，用于视频封面或者音乐封面；
- `$media->getStream($mediaId);` 获取临时素材内容，比如图片、视频、声音等二进制流内容；
- `$media->download($mediaId, $directory, $filename);` 下载临时素材到本地，`$directory` 为目标目录，`$filename` 为新的文件名，可以为空，默认使用 `$mediaId` 作为文件名。


更多请参考 [微信官方文档](http://mp.weixin.qq.com/wiki/home/index.html) `素材管理` 章节