# 配置

常用的配置参数会比较少，因为除非你有特别的定制，否则基本上默认值就可以了：

```php
use EasyWeChat\Factory;

$config = [
    'app_id' => 'wx3cf0f39249eb0exx',
    'secret' => 'f1c242f4f28f735d4687abb469072axx',

    // 指定 API 调用返回结果的类型：array(default)/collection/object/raw/自定义类名
    'response_type' => 'array',

    'log' => [
        'level' => 'debug',
        'file' => __DIR__.'/wechat.log',
    ],
];

$app = Factory::officialAccount($config);
```

下面是一个完整的配置样例：

> 不建议你在配置的时候弄这么多，用到啥就配置啥才是最好的，因为大部分用默认值即可。

```php
<?php

return [
    /**
     * 账号基本信息，请从微信公众平台/开放平台获取
     */
    'app_id'  => 'your-app-id',         // AppID
    'secret'  => 'your-app-secret',     // AppSecret
    'token'   => 'your-token',          // Token
    'aes_key' => '',                    // EncodingAESKey，兼容与安全模式下请一定要填写！！！

     /**
      * 指定 API 调用返回结果的类型：array(default)/collection/object/raw/自定义类名
      * 使用自定义类名时，构造函数将会接收一个 `EasyWeChat\Kernel\Http\Response` 实例
      */
    'response_type' => 'array',

    /**
     * 日志配置
     *
     * level: 日志级别, 可选为：
     *         debug/info/notice/warning/error/critical/alert/emergency
     * path：日志文件位置(绝对路径!!!)，要求可写权限
     */
    'log' => [
        'default' => 'dev', // 默认使用的 channel，生产环境可以改为下面的 prod
        'channels' => [
            // 测试环境
            'dev' => [
                'driver' => 'single',
                'path' => '/tmp/easywechat.log',
                'level' => 'debug',
            ],
            // 生产环境
            'prod' => [
                'driver' => 'daily',
                'path' => '/tmp/easywechat.log',
                'level' => 'info',
            ],
        ],
    ],

    /**
     * 接口请求相关配置，超时时间等，具体可用参数请参考：
     * http://docs.guzzlephp.org/en/stable/request-config.html
     *
     * - retries: 重试次数，默认 1，指定当 http 请求失败时重试的次数。
     * - retry_delay: 重试延迟间隔（单位：ms），默认 500
     * - log_template: 指定 HTTP 日志模板，请参考：https://github.com/guzzle/guzzle/blob/master/src/MessageFormatter.php
     */
    'http' => [
        'max_retries' => 1,
        'retry_delay' => 500,
        'timeout' => 5.0,
        // 'base_uri' => 'https://api.weixin.qq.com/', // 如果你在国外想要覆盖默认的 url 的时候才使用，根据不同的模块配置不同的 uri
    ],

    /**
     * OAuth 配置
     *
     * scopes：公众平台（snsapi_userinfo / snsapi_base），开放平台：snsapi_login
     * callback：OAuth授权完成后的回调页地址
     */
    'oauth' => [
        'scopes'   => ['snsapi_userinfo'],
        'callback' => '/examples/oauth_callback.php',
    ],
];
```

> :heart: 安全模式下请一定要填写 `aes_key`

## 日志配置

日志有两种配置方式：

### 文件式

```php
'log' => [
    'level'      => 'debug',
    'permission' => 0777,
    'file'       => '/tmp/easywechat.log',
],
```

配置文件里的 `/tmp/...` 是绝对路径

如果在 windows 平台，需要把它改成 `C:\foo\bar` 的形式，

如果需要按日独立存储，可以配置成 `'file'  => storage_path('/tmp/easywechat/easywechat_'.date('Ymd').'.log'),`

### 自定义 Handler

由于日志使用的是 [Monolog](https://github.com/Seldaek/monolog)，所以，除了默认的文件式日志外，你可以自定义日志处理器：

```php
use Monolog\Handler\RotatingFileHandler;

$handler = new RotatingFileHandler('/path/to/wechat.log', 5, 'debug');

...

'log' => [
    'level'   => 'debug',
    'handler' => $handler,
],
```

