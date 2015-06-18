title: 消息
---


消息的监听再简单不过了，你不需要像其它 SDK 一样麻烦，这将会是前所未有的简单，你可以选择监听所有类型或者指定某种类型，以及作出相应的响应，比如回复一条应答消息。

在本 SDK 中，服务端的操作，例如：服务器认证，监听事件，接收用户消息等所有微信向我们自有服务器发起请求的，都交由 `Overtrue\Wechat\Server` 来处理。

### 获取服务端实例

```php
<?php
use Overtrue\Wechat\Server;

$appId          = 'wx3cf0f39249eb0e60';
$token          = 'hellotest';
$encodingAESKey = 'EJThPazwzO4k1cyXJnwQtL60zBdhWvFaHb4emv0dLVN'; // 可选

// $encodingAESKey 可以为空
$server = new Server($appId, $token, $encodingAESKey);
```

### 接收消息

#### 语法：

```php
$wechat->on('message', callable $callback); // 全部类型
// or
$wechat->on('message', string $messageType, callable $callback); // 只监听指定类型
```

#### 参数说明：

- `$messageType` string, 指定要处理的消息类型，ex：`image`
- `$callback` callable, 回调函数，closure 匿名函数，或者一切可调用的方法或者函数

`$callback` 接收一个参数：`$message` 为用户发送的消息对象，你可以访问[请求消息的所有属性](https://github.com/overtrue/wechat/wiki/%E6%B6%88%E6%81%AF%E7%9A%84%E4%BD%BF%E7%94%A8#%E8%AF%B7%E6%B1%82%E6%B6%88%E6%81%AF%E7%9A%84%E5%B1%9E%E6%80%A7)，比如：`$message->FromUserName` 得到发送消息的人的 `openid`，`$message->MsgType` 获取消息的类型如 `text` 或者 `image` 等，更多请参考：[请求消息的属性](https://github.com/overtrue/wechat/wiki/%E6%B6%88%E6%81%AF%E7%9A%84%E4%BD%BF%E7%94%A8#%E8%AF%B7%E6%B1%82%E6%B6%88%E6%81%AF%E7%9A%84%E5%B1%9E%E6%80%A7)

example:

```php
<?php

use Overtrue\Wechat\Server;
use Overtrue\Wechat\Message;


$appId          = 'wx3cf0f39249eb0e60';
$secret         = 'f1c242f4f28f735d4687abb469072a29';
$token          = 'hellotest';
$encodingAESKey = 'EJThPazwzO4k1cyXJnwQtL60zBdhWvFaHb4emv0dLVN';

$server = new Server($appId, $token, $encodingAESKey);

// 监听所有类型
$server->on('message', function($message) {
    return Message::make('text')->content('您好！');
});

// 监听指定类型
$server->on('message', 'image', function($message) {
    return Message::make('text')->content('我们已经收到您发送的图片！');
});

$result = $server->serve();

echo $result;
```


我把微信的 API 里的所有“消息”都按类型抽象出来了，也就是说，你不用区分它是回复消息还是主动推送消息，免去了你去手动拼装微信那帮 SB 那么恶心的 XML 以及乱七八糟命名不统一的 JSON 了，我替你承受这份苦，不要问是谁，我是雷锋他弟弟，雷管。

### 请求消息的属性

当你接收到用户发来的消息时，可能会提取消息中的相关属性，那么请参考：

请求消息基本属性：

    ToUserName    接收方帐号（该公众号 ID）
    FromUserName  发送方帐号（代表用户的唯一标识）
    CreateTime    消息创建时间（时间戳）
    MsgId         消息 ID（64位整型）

文本消息请求：

    MsgType  text
    Content  文本消息内容

图片消息请求：

    MsgType  image
    PicUrl   图片链接

地理位置消息请求：

    MsgType     location
    Location_X  地理位置纬度
    Location_Y  地理位置经度
    Scale       地图缩放大小
    Label       地理位置信息

链接消息请求：

    MsgType      link
    Title        消息标题
    Description  消息描述
    Url          消息链接

### 回复消息的类型及属性（创建回复时用到）

| 消息类型 | 类型名称 | 属性  | 除属性自身外提供的方法 |
|----------|----------|-----|--------------------|
| 文本     | `text`     | `content` 内容     |  |
| 图片     | `image`    | `media_id` 媒体资源id  | `media($mediaId)`   |
| 声音     | `voice`    | `media_id` 媒体资源id   | `media($mediaId)` |
| 音乐     | `music`    | `title` 标题 <br>`description` 描述 <br>`url` 音乐URL <br>`hq_url` 高清URL <br>`thumb_media_id` 封面资源id | `thumb($mediaId)` |
| 视频     | `video`    | `title` 标题 <br>`description` 描述 <br>`media_id` 媒体资源id <br>`thumb_media_id` 封面资源id        | `media($mediaId)` <br>`thumb($mediaId)`  |
| 位置     | `location` | `lat` 地理位置纬度 <br>`lon` 地理位置经度 <br>`scale` 地图缩放大小 <br>`label` 地理位置信息 |  |
| 链接     | `link`     | `title` 标题 <br>`description` 描述<br>url  链接URL  | |
| 图文*     | `news(news_item)`     | 'title'  标题 <br>  'description'  描述<br> 'pic_url' 图片链接 <br> 'url' 链接URL | |

> 注意：图文消息结构与其它不一样，具体请参考下面的示例部分。

### 创建消息

**请注意：消息工厂类的命名空间为 `Overtrue\Wechat\Message`**

```php
<?php

use Overtrue\Wechat\Server;
use Overtrue\Wechat\Message;

$appId          = 'wx3cf0f39249eb0e60';
$token          = 'hellotest';
$encodingAESKey = 'EJThPazwzO4k1cyXJnwQtL60zBdhWvFaHb4emv0dLVN';

$server = new Server($appId, $token, $encodingAESKey);

$server->on('event', 'subscribe', function($event){
  return Message::make('text')->content('您好！欢迎关注 overtrue');
});
```

这里有一点需要注意，当属性带下划线的时候，方法名是支持两种的：`media_id()` 或者 `mediaId()` 都一样。

### 上传素材文件

```php
<?php
use Overtrue\Wechat\Media;
// $appId = xxxx;
// $appSecret = xxxx;
// ...
$media = new Media($appId, $appSecret);
$imageId = $media->image(__DIR__ . '/demo.jpg'); // 上传并返回媒体ID

$message = Message::make('image')->media($imageId);
```


> 请注意：方法`media($mediaId)`会直接赋值到`media_id`属性。不会自动上传文件。1.x 版本是会自动上传的，2.x 已经不再提供。

#### 这里有两个方法用于设置素材ID：

- `media($mediaId)` 对应设置 `media_id`
- `thumb($mediaId)` 对应设置 `thumb_media_id`

### 图文消息

图文消息比较不一样的是他是包含多个 item 的结构体，那么这里需要使用 `Message::make('news_item')` 来得到消息 item：

```php
$news = Message::make('news')->items(function(){
    return array(
            Message::make('news_item')->title('测试标题'),
            Message::make('news_item')->title('测试标题2')->description('好不好？'),
            Message::make('news_item')->title('测试标题3')->description('好不好说句话？')->url('http://baidu.com'),
            Message::make('news_item')->title('测试标题4')->url('http://baidu.com/abc.php')->picUrl('http://www.baidu.com/demo.jpg'),
           );
});
```

关于更多素材管理请参考：[素材管理](素材管理)