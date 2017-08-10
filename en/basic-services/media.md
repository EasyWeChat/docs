# 临时素材 API

上传的临时多媒体文件有格式和大小限制，如下：

- 图片（image）: 1M，支持 `JPG` 格式
- 语音（voice）：2M，播放长度不超过 `60s`，支持 `AMR\MP3` 格式
- 视频（video）：10MB，支持 `MP4` 格式
- 缩略图（thumb）：64KB，支持 `JPG` 格式

## 上传图片

> 注意：微信图片上传服务有敏感检测系统，图片内容如果含有敏感内容，如色情，商品推广，虚假信息等，上传可能失败。

```php
$temporary->uploadImage($path);
```

## 上传声音

```php
$temporary->uploadVoice($path);
```

## 上传视频

```php
$temporary->uploadVideo($path, $title, $description);
```

## 上传缩略图

用于视频封面或者音乐封面。

```php
$temporary->uploadThumb($path);
```

## 获取临时素材内容

比如图片、视频、声音等二进制流内容。

```php
$content = $temporary->getStream($mediaId);
file_put_contents('/tmp/abc.jpg', $content);// 请使用绝对路径写法！除非你正确的理解了相对路径（好多人是没理解对的）！
```

## 下载临时素材到本地

其实就是上一个 API 的封装。

```php
$temporary->download($mediaId, "/tmp/", "abc.jpg");
```

参数说明：

  - `$directory` 为目标目录，
  - `$filename` 为新的文件名，可以为空，默认使用 `$mediaId` 作为文件名。


更多请参考 [微信官方文档](http://mp.weixin.qq.com/wiki) `素材管理` 章节