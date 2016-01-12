title: 配置
---

在前面我们已经讲过，初始化 SDK 的时候方法就是创建一个 `EasyWeChat\Foundation\Application` 实例：

```php
use EasyWeChat\Foundation\Application;

$options = [
   // ...
];

$app = new Application($options);
```

那么配置的具体选项有哪些，下面是一个完整的列表：

```php
<?php

return [
    /**
     * Debug 模式，bool 值：true/false
     *
     * 当值为 false 时，所有的日志都不会记录
     */
    'debug'  => true,

    /**
     * 账号基本信息，请从微信公众平台/开放平台获取
     */
    'app_id'  => 'your-app-id',         // AppID
    'secret'  => 'your-app-secret',     // AppSecret
    'token'   => 'your-token',          // Token
    'aes_key' => '',                    // EncodingAESKey

    /**
     * 日志配置
     *
     * level: 记录的级别,\Monolog\Logger 常量，可选为：
     *         debug/info/notice/warning/error/critical/alert/emergency
     * file：日志文件位置，要求可写权限
     */
    'log' => [
        'level' => 'debug',
        'file'  => '/tmp/easywechat.log',
    ],

    /**
     * OAuth 配置
     *
     * scopes：公众平台（snsapi_base/snsapi_base），开放平台：snsapi_login
     * callback：OAuth授权完成后的回调页地址
     */
    'oauth' => [
        'scopes'   => ['snsapi_userinfo'],
        'callback' => '/examples/oauth_callback.php',
    ],

    // more...
];
```