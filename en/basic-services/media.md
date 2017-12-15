# Temporary material API

Uploaded temporary multimedia files have format and size restrictions as follows：

- Image : 1M，support `JPG` format
- Voice ：2M，play length does not exceed `60s`，support `AMR\MP3` format
- Video ：10MB，support `MP4` format
- Thumb ：64KB，support `JPG` format

## Upload Image

> Note：WeChat image upload service has a sensitive detection system. If the image content contains sensitive content such as pornography， product promotion， fake information，etc， the upload may fail。

```php
$temporary->uploadImage($path);
```

## Upload Voice

```php
$temporary->uploadVoice($path);
```

## Upload Video

```php
$temporary->uploadVideo($path, $title, $description);
```

## Upload Thumb

For video cover or music cover.

```php
$temporary->uploadThumb($path);
```

## Get Temporary Material

Such as pictures，video， audio and other binary stream content.

```php
$content = $temporary->getStream($mediaId);
file_put_contents('/tmp/abc.jpg', $content);// Please use the absolute path method! Unless you correctly understand the relative path (a lot of people do not understand the method)!
```

## Download Temporary Material

```php
$temporary->download($mediaId, "/tmp/", "abc.jpg");
```

Params：

  - `$directory` target directory，
  - `$filename` new filename, which can be null, defaults to `$ mediaId` as the filename。


More use please refer to [WeChat official document](http://mp.weixin.qq.com/wiki/) in the "Material Management" chapter。