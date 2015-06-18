title: Configuration
---

This project contains several APIs, each of them may have a defferent initial functional parameters.

All of classes had been also provided the `class_alias`, list its below: *tip: while using the alias classes, you may enable the following line onto your project's boot script.


```php
Overtrue\Wechat\Alias::register();
```

If you are favorited While using the `class_alias`, it won't need import the namespace again.

### Server

    - class: `Overtrue\Wechat\Server`
    - alias: `WechatServer`
    - parameters: `__construct($appId, $token, $encodingAESKey = null)`

### Message

    - class: `Overtrue\Wechat\Message`
    - alias: `WechatMessage`
    - parameters: none

### User

    - class: `Overtrue\Wechat\User`
    - alias: `WechatUser`
    - parameters: `__construct($appId, $appSecret)`

### Group

    - class: `Overtrue\Wechat\Group`
    - alias: `WechatGroup`
    - parameters: `__construct($appId, $appSecret)`

### Web Auth

    - class: `Overtrue\Wechat\Auth`
    - alias: `WechatAuth`
    - parameters: `__construct($appId, $appSecret)`

### Menu

    - class: `Overtrue\Wechat\Menu`
    - alias: `WechatMenu`
    - parameters: `__construct($appId, $appSecret)`

### Menu Items

    - class: `Overtrue\Wechat\MenuItem`
    - alias: `WechatMenuItem`
    - parameters: `__construct($appId, $appSecret)`

### JSSDK

    - class: `Overtrue\Wechat\Js`
    - alias: `WechatJs`
    - parameters: `__construct($appId, $appSecret)`

### Customer Service(aka: Hotline)

    - class: `Overtrue\Wechat\Staff`
    - alias: `WechatStaff`
    - parameters: `__construct($appId, $appSecret)`

### Store

    - class: `Overtrue\Wechat\Store`
    - alias: `WechatStore`
    - parameters: `__construct($appId, $appSecret)`

### WeChat Card

    - class: `Overtrue\Wechat\Card`
    - alias: `WechatCard`
    - parameters: `__construct($appId, $appSecret)`

### QR Code

    - class: `Overtrue\Wechat\QRCode`
    - alias: `WechatQRCode`
    - parameters: `__construct($appId, $appSecret)`

### Short URL Service

    - class: `Overtrue\Wechat\Url`
    - alias: `WechatUrl`
    - parameters: `__construct($appId, $appSecret)`

### Multimedia resources Management

    - class: `Overtrue\Wechat\Media`
    - alias: `WechatMedia`
    - parameters: `__construct($appId, $appSecret)`

### Image

    - class: `Overtrue\Wechat\Image`
    - alias: `WechatImage`
    - parameters: `__construct($appId, $appSecret)`

### Exception

    - class: `Overtrue\Wechat\Exception`
    - alias: `WechatException`
    - parameters: `__construct($appId, $appSecret)`

### BaseMessage

    - class: `Overtrue\Wechat\BaseMessage`
    - alias: `WechatBaseMessage`
    - parameters: `__construct($appId, $appSecret)`

### Message:Text

    - class: `Overtrue\Wechat\Messages\Text`
    - alias: `WechatTextMessage`
    - parameters: None

### Message:Image

    - class: `Overtrue\Wechat\Messages\Image`
    - alias: `WechatImageMessage`
    - parameters: None

### Message:Audio

    - class: `Overtrue\Wechat\Messages\Voice`
    - alias: `WechatVoiceMessage`
    - parameters: None

### Message:Video

    - class: `Overtrue\Wechat\Messages\Video`
    - alias: `WechatVideoMessage`
    - parameters: None

### Message:News

    - class: `Overtrue\Wechat\Messages\News`
    - alias: `WechatNewsMessage`
    - parameters: None

### Message:NewsItem

    - class: `Overtrue\Wechat\Messages\NewsItem`
    - alias: `WechatNewsMessageItem`
    - parameters: None

### Message:Location

    - class: `Overtrue\Wechat\Messages\Location`
    - alias: `WechatLocationMessage`
    - parameters: None

### Message:Music

    - class: `Overtrue\Wechat\Messages\Music`
    - alias: `WechatMusicMessage`
    - parameters: None

### Message:Link

    - class: `Overtrue\Wechat\Messages\Link`
    - alias: `WechatLinkMessage`
    - parameters: None

### Message:Transfer(Mandatary for the Hotline Service)

    - class: `Overtrue\Wechat\Messages\Transfer`
    - alias: `WechatTransferMessage`
    - parameters: None


About the PHP namespace, @see [PHP Offical](http://php.net/manual/en/language.namespaces.php)