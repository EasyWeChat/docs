title: 配置
---

本项目中提供了很多服务，每个服务需要的初始化参数可能会有不同：

所有对外开发的类基本都有别名，列表如下，别名默认不启用，如需要启用别名，请：

```php
Overtrue\Wechat\Alias::register();
```

使用别名就不需要引入命名空间了，对于命名空间不熟悉的同学可以选用别名方式使用。

### 服务端

    - 完整类名：`Overtrue\Wechat\Server`
    - 别名：`WechatServer`
    - 参数：`__construct($appId, $token, $encodingAESKey = null)`

### 消息

    - 完整类名：`Overtrue\Wechat\Message`
    - 别名：`WechatMessage`
    - 参数：无

### 用户

    - 完整类名：`Overtrue\Wechat\User`
    - 别名：`WechatUser`
    - 参数：`__construct($appId, $appSecret)`

### 用户组

    - 完整类名：`Overtrue\Wechat\Group`
    - 别名：`WechatGroup`
    - 参数：`__construct($appId, $appSecret)`

### 网页授权

    - 完整类名：`Overtrue\Wechat\Auth`
    - 别名：`WechatAuth`
    - 参数：`__construct($appId, $appSecret)`

### 菜单

    - 完整类名：`Overtrue\Wechat\Menu`
    - 别名：`WechatMenu`
    - 参数：`__construct($appId, $appSecret)`

### 菜单项

    - 完整类名：`Overtrue\Wechat\MenuItem`
    - 别名：`WechatMenuItem`
    - 参数：`__construct($appId, $appSecret)`

### JSSDK

    - 完整类名：`Overtrue\Wechat\Js`
    - 别名：`WechatJs`
    - 参数：`__construct($appId, $appSecret)`

### 客服

    - 完整类名：`Overtrue\Wechat\Staff`
    - 别名：`WechatStaff`
    - 参数：`__construct($appId, $appSecret)`

### 门店

    - 完整类名：`Overtrue\Wechat\Store`
    - 别名：`WechatStore`
    - 参数：`__construct($appId, $appSecret)`

### 卡券

    - 完整类名：`Overtrue\Wechat\Card`
    - 别名：`WechatCard`
    - 参数：`__construct($appId, $appSecret)`

### 二维码

    - 完整类名：`Overtrue\Wechat\QRCode`
    - 别名：`WechatQRCode`
    - 参数：`__construct($appId, $appSecret)`

### URL

    - 完整类名：`Overtrue\Wechat\Url`
    - 别名：`WechatUrl`
    - 参数：`__construct($appId, $appSecret)`

### 素材

    - 完整类名：`Overtrue\Wechat\Media`
    - 别名：`WechatMedia`
    - 参数：`__construct($appId, $appSecret)`

### 图片

    - 完整类名：`Overtrue\Wechat\Image`
    - 别名：`WechatImage`
    - 参数：`__construct($appId, $appSecret)`

### 异常

    - 完整类名：`Overtrue\Wechat\Exception`
    - 别名：`WechatException`
    - 参数：`__construct($appId, $appSecret)`

### 消息基类

    - 完整类名：`Overtrue\Wechat\BaseMessage`
    - 别名：`WechatBaseMessage`
    - 参数：`__construct($appId, $appSecret)`

### 文本消息

    - 完整类名：`Overtrue\Wechat\Messages\Text`
    - 别名：`WechatTextMessage`
    - 参数：无

### 图片消息

    - 完整类名：`Overtrue\Wechat\Messages\Image`
    - 别名：`WechatImageMessage`
    - 参数：无

### 声音消息

    - 完整类名：`Overtrue\Wechat\Messages\Voice`
    - 别名：`WechatVoiceMessage`
    - 参数：无

### 视频消息

    - 完整类名：`Overtrue\Wechat\Messages\Video`
    - 别名：`WechatVideoMessage`
    - 参数：无

### 图文消息

    - 完整类名：`Overtrue\Wechat\Messages\News`
    - 别名：`WechatNewsMessage`
    - 参数：无

### 图文消息项

    - 完整类名：`Overtrue\Wechat\Messages\NewsItem`
    - 别名：`WechatNewsMessageItem`
    - 参数：无

### 坐标消息

    - 完整类名：`Overtrue\Wechat\Messages\Location`
    - 别名：`WechatLocationMessage`
    - 参数：无

### 音乐消息

    - 完整类名：`Overtrue\Wechat\Messages\Music`
    - 别名：`WechatMusicMessage`
    - 参数：无

### 链接消息

    - 完整类名：`Overtrue\Wechat\Messages\Link`
    - 别名：`WechatLinkMessage`
    - 参数：无

### 客服转发消息

    - 完整类名：`Overtrue\Wechat\Messages\Transfer`
    - 别名：`WechatTransferMessage`
    - 参数：无


请在使用中不要忘记引入命名空间。关于命名空间的使用：[PHP官方文档](http://php.net/manual/zh/language.namespaces.php)