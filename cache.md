title: 缓存
---

本项目使用 [doctrine/cache](https://github.com/doctrine/cache) 来完成缓存工作，它支持基本目前所有的缓存引擎。

在我们的 SDK 中的所有缓存默认使用文件缓存，缓存路径取决于 PHP 的临时目录，如果你需要自定义缓存，那么你需要做如下的事情：

你可以参考[doctrine/cache官方文档](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/caching.html)来替换掉应用中默认的缓存配置：

> 以 redis 为例
> 请先安装 redis 拓展：https://github.com/phpredis/phpredis

```php

use Doctrine\Common\Cache\RedisCache;

$cacheDriver = new RedisCache();

// 创建 redis 实例
$redis = new Redis();
$redis->connect('redis_host', 6379);

$cacheDriver->setRedis($redis);

$options = [
    'debug'  => false,
    'app_id' => $wechatInfo['app_id'],
    'secret' => $wechatInfo['app_secret'],
    'token'  => $wechatInfo['token'],
    'aes_key' => $wechatInfo['aes_key'], // 可选
    'cache'   => $cache,
];

$wechatApp = new Application($options);
```

### Laravel 中使用

在 Laravel 中框架使用 [predis/predis](https://github.com/nrk/predis)，那么我们就得使用 `Doctrine\Common\Cache\PredisCache`：

```php

use Doctrine\Common\Cache\PredisCache;

$predis = app('redis')->connection();// connection($name), $name 默认为 `default`
$cacheDriver = new PredisCache($predis);

$app->cache = $cacheDriver;
```

> 上面提到的 `app('redis')->connection($name)`, 这里的 `$name` 是 laravel 项目中配置文件 `database.php` 中 `redis` 配置名 `default`：https://github.com/laravel/laravel/blob/master/config/database.php#L118
> 如果你使用的其它连接，对应传名称就好了。
