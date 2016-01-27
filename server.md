title: 服务端
---

我们在入门小教程一节以服务端为例讲解了一个基本的消息的处理，这里就不再讲服务器验证的流程了，请直接参考前面的入门实例即可。

服务端的作用呢，在整个微信开发中主要是负责 **[接收用户发送过来的消息](http://mp.weixin.qq.com/wiki/10/79502792eef98d6e0c6e1739da387346.html)**，还有 **[用户触发的一系列事件](http://mp.weixin.qq.com/wiki/2/5baf56ce4947d35003b86a9805634b1e.html)**。

首先我们得厘清一下消息与事件的回复，当你收到用户消息后（消息由微信服务器推送到你的服务器），在你对消息进行一些处理后，不管是选择回复一个消息还是什么不都回给用户，你也应该给微信服务器一个 “答复”，如果是选择回复一条消息，就直接返回一个消息xml就好，如果选择不作任何回复，你也得回复一个空字符串或者字符串 `SUCCESS`（不然用户就会看到 `该公众号暂时无法提供服务`）。

## 基本使用

在 SDK 中呢，使用 `setMessageHandler(callable $callback)` 来设置消息处理函数：

```php
<?php
use EasyWeChat\Foundation\Application;

// ...

$app = new Application($options);

// 从项目实例中得到服务端应用实例。
$server = $app->server;

$server->setMessageHandler(function ($message) {
    return "您好！欢迎关注我!";
});

$server->serve()->send();
```

这里我们使用 `setMessageHandler` 传入了一个 **闭包（[Closure](http://php.net/manual/en/class.closure.php)）**，该闭包接收一个参数 `$message` 为消息对象（Collection），这里需要注意的时，与 2.0 不同，2.0 当中我们对消息与事件做了区分，还对消息进行了分类（按 MsgType）。在 3.0 后，所有的消息包括事件都会使用 `setMessageHandler` 来处理，也就是说你可能需要在里面进行一些判断，例如：

```php
$server->setMessageHandler(function ($message) {
    switch ($message->MsgType) {
        case 'event':
            # 事件消息...
            break;
        case 'text':
            # 文字消息...
            break;
        case 'image':
            # 图片消息...
            break;
        case 'voice':
            # 语音消息...
            break;
        // ... 其它消息
        default:
            # code...
            break;
    }

    // ...
});
```

当然，因为这里 `setMessageHandler` 接收一个 [`callable`](http://php.net/manual/zh/language.types.callable.php) 的参数，所以你不一定要传入一个 Closure 闭包，你可以选择传入一个函数名，一个 `[$class, $method]` 或者 `Foo::bar` 这样的类型。

## 请求消息的属性

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

## 回复消息

回复的消息可以为 `null`，此时 SDK 会返回给微信一个 "SUCCESS"，你也可以回复一个普通字符串，比如：`欢迎关注 overtrue.`，此时 SDK 会对它进行一个封装，产生一个 [`EasyWeChat\Message\Text`](https://github.com/EasyWeChat/message/blob/master/src/Text.php) 类型的消息并在最后的 `$server->serve();` 时生成对应的消息 XML 格式。

如果你想返回一个自己手动拼的原生 XML 格式消息，请返回一个 [`EasyWeChat\Message\Raw`](https://github.com/EasyWeChat/message/blob/master/src/Raw.php) 实例即可。

关于消息的使用，请参考 [`消息`](docs/messages.html) 章节。
