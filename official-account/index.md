# 公众号

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

更多配置项请参考：[配置](config.md)

## API

Application 就是一个工厂类，所有的模块都是从 `$app` 中访问，并且几乎都提供了协议和 setter 可自定义修改。

### 服务端

服务端模块封装了服务端相关的便捷操作，隐藏了部分复杂的细节，基于中间件模式可以更方便的处理消息推送和服务端验证。

```php
$app->getServer();
```

更多说明请参阅：[服务端使用文档](server.md)

### API Client

封装了多种模式的 API 调用类，你可以选择自己喜欢的方式调用公众号任意 API，默认自动处理了 access_token 相关的逻辑。

```php
$app->getClient();
```

更多说明请参阅：[API调用](client.md)

### 配置

```php
$config = $app->getConfig();
```

你可以轻松使用 `$config->get($key, $default)` 或 `$config->set($key, $value)` 在调用前修改配置项。

### AccessToken

access_token 是公众号 API 调用的必备条件，如果你想获取它的值，你可以通过以下方式拿到当前的 access_token：

```php
$accessToken = $app->getAccessToken();
$accessToken->getToken(); // string
```

当然你也可以使用自己的 AccessToken 类：

```php
$accessToken = new MyCustomAccessToken();
$app->setAccessToken($accessToken)
```

### 公众号账户

公众号账号类，提供一系列 API 获取公众号的基本信息：

```php
$app->getAccount();
```