# API 调用

与以往版本不同的是，SDK 不再内置具体 API 的逻辑，所有的 API 均交由开发者自行调用，以获取用户列表为例：

```php
$api = $app->getClient();
```

## 链式调用

你可以将需要调用的 API 以 `/` 分割 + 驼峰写法的形式，写成如下模式：

```php
$users = $api->cgiBin->user->get->get()->toArray();
```

它最终就是转化为：

```
GET /cgi-bin/user/get
```

### 链式转换规则

- 请求 path 中的 `/` 为分隔符，切割成属性，例如：`/cgi-bin/user/info/updateremark` 则转换成 `->cgiBin->user->info->updateremark`；
- path 对应的请求方法（HTTP Method），即作为请求对象的末尾执行方法，例如: `->cgiBin->user->info->updateremark->post([...])`；
- 有中横线分隔符(`-`)的，可以使用驼峰（camelCase）风格书写，例如: merchant-service 可写成 merchantService；
- 动态参数，例如 `business_code/{business_code}` 可写成 `->businessCode[11029999911]`，或按属性风格，直接写值也可以，例如 `businessCode->{2000001234567890}`；

> :heart: 链式调用参考自朋友 `TheNorthMemory` 的插件 [TheNorthMemory/wechatpay-axios-plugin](https://github.com/TheNorthMemory/wechatpay-axios-plugin) 中的创意。

### 参数传递

#### GET

你可以在最后的调用方法里传递对应的参数，例如：

```php
$users = $api->cgiBin->user->get->get([
    'query' => [
            'next_openid' => 'OPENID1',
        ]
    ])->toArray();
```

#### POST

```php
$api->cgiBin->user->info->updateremark->post([
    'body' => [
            "openid" => "oDF3iY9ffA-hqb2vVvbr7qxf6A0Q",
            "remark" => "pangzi"
        ]
    ])->toArray();
```

或者指定 json 格式：

```php
$api->cgiBin->user->info->updateremark->post([
    'json' => [
            "openid" => "oDF3iY9ffA-hqb2vVvbr7qxf6A0Q",
            "remark" => "pangzi"
        ]
    ])->toArray();
```

## 原始方式调用

如果你不喜欢链式转换的调用方式，你还可以选择以传统方式调用：


```php
$response = $api->post('/cgi-bin/user/info/updateremark', ['body' => [
    "openid" => "oDF3iY9ffA-hqb2vVvbr7qxf6A0Q",
    "remark" => "pangzi"
]]);
```

### 语法说明

```php
Psr\Http\Message\ResponseInterface {get/post/patch/put/delete}($uri, $options = [])
```

**参数说明：**

- `$uri` 为需要请求的 `path`；
- `$options` 为请求参数，可以指定 `query` / `body` / `headers` 等等，具体请参考：[Symfony\Contracts\HttpClient\HttpClientInterface::OPTIONS_DEFAULTS](https://github.com/symfony/symfony/blob/5.3/src/Symfony/Contracts/HttpClient/HttpClientInterface.php)

## 处理响应

API Client 基于 [symfony/http-client](https://github.com/symfony/http-client) 实现，你可以通过以下方式对响应值进行访问：

```php
$response = $api->cgiBin->user->get->get();

// 获取状态码
$statusCode = $response->getStatusCode();

// 获取全部响应头
$headers = $response->getHeaders();

// 获取响应原始内容
$content = $response->getContent();

// 获取 json 转换后的数组格式
$content = $response->toArray();

// 将内容转换成 Stream 返回
$content = $response->toStream();

// 获取其他信息，如："response_headers", "redirect_count", "start_time", "redirect_url" 等.
$httpInfo = $response->getInfo();

// 获取指定信息
$startTime = $response->getInfo('start_time');

// 获取请求日志
$httpLogs = $response->getInfo('debug');
```

:book: 更多使用请参考： [HTTP client: Processing Responses](https://symfony.com/doc/current/http_client.html#processing-responses)



## 异步请求

todo