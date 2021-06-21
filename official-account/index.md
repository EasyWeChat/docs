# 使用配置创建应用

常用的配置参数会比较少，因为除非你有特别的定制，否则基本上默认值就可以了：

```php
use EasyWeChat\OfficialAccount\Application;

$config = [
    'app_id' => 'wx3cf0f39249eb0exx',
    'secret' => 'f1c242f4f28f735d4687abb469072axx',
    'token' => 'easywechat',
    'aes_key' => '......'
    //...
];

$app = new Application($config);
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
     * https://github.com/symfony/symfony/blob/5.3/src/Symfony/Contracts/HttpClient/HttpClientInterface.php
     */
    'http' => [
        'timeout' => 5.0,
        // 'base_uri' => 'https://api.weixin.qq.com/', // 如果你在国外想要覆盖默认的 url 的时候才使用，根据不同的模块配置不同的 uri
    ],
];
```

> :heart: 安全模式下请一定要填写 `aes_key`

## 日志配置

你可以配置多个日志的 channel，每个 channel 里的 `driver` 对应不同的日志驱动，内置可用的 `driver` 如下表：

名称 | 描述
------------- | -------------
`stack` | 复合型，可以包含下面多种驱动的混合模式
`single` | 基于 `StreamHandler` 的单一文件日志，参数有 `path`，`level`
`daily` | 基于 `RotatingFileHandler` 按日期生成日志文件，参数有 `path`，`level`，`days`(默认 7 天)
`slack` | 基于 `SlackWebhookHandler` 的 Slack 组件，参数请参考源码：[LogManager.php](https://github.com/w7corp/wechat/blob/master/src/Kernel/Log/LogManager.php#L247)
`syslog` | 基于 `SyslogHandler` Monolog 驱动，参数有 `facility` 默认为 `LOG_USER`，`level`
`errorlog` | 记录日志到系统错误日志，基于 `ErrorLogHandler`，参数有 `type`，默认为 `ErrorLogHandler::OPERATING_SYSTEM`

### 自定义日志驱动

由于日志使用的是 [Monolog](https://github.com/Seldaek/monolog)，所以，除了默认的文件式日志外，你可以自定义日志处理器：

```php
use Monolog\Logger;
use Monolog\Handler\RotatingFileHandler;


// 注册自定义日志
$app->getLogger()->extend('mylog', function($app, $config){
    return new Logger($this->parseChannel($config), [
        $this->prepareHandler(new RotatingFileHandler(
            $config['path'], $config['days'], $this->level($config)
        )),
    ]);
});
```

> {info} 在你自定义的闭包函数中，可以使用 `EasyWeChat\Kernel\Log\LogManager` 中的方法，具体请查看 SDK 源代码。

配置文件中在 `driver` 部分即可使用你自定义的驱动了：

```php
'log' => [
    'default' => 'dev', // 默认使用的 channel，生产环境可以改为下面的 prod
    'channels' => [
        // 测试环境
        'dev' => [
            'driver' => 'mylog',
            'path' => '/tmp/easywechat.log',
            'level' => 'debug',
            'days' => 5,
        ],

        //...
    ],
],
```

