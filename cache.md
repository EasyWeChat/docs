title: 缓存
---

在我们的 SDK 中的所有缓存默认使用文件缓存，缓存路径取决于 PHP 的临时目录，如果你需要自定义缓存，那么你需要做如下的事情：

创建自己的缓存适配器：

```php
<?php

use EasyWeChat\Cache\Adapters\AdapterInterface;

class MyCustomCacheAdapter implements AdapterInterface
{
    /**
         * Set app id.
         *
         * @param string $appId
         *
         * @return AdapterInterface
         */
        public function setAppId($appId)
        {
            // ...
        }

        /**
         * Get cache content.
         *
         * @param string     $key
         * @param mixed|null $default
         *
         * @return string
         */
        public function get($key, $default = null)
        {
            // ...
        }

        /**
         * Set cache content.
         *
         * @param string $key
         * @param string $value
         * @param int    $lifetime
         *
         * @return int
         */
        public function set($key, $value, $lifetime = 7200)
        {
            // ...
        }
}
```

然后替换掉应用中默认的缓存配置：

```php

use EasyWeChat\Cache\Manager as CacheManager;

$app['cache']->setAdapter(new MyCustomCacheAdapter());
```

- `setAppId($appId)` 的作用呢，一是用于区别不同账号的缓存，二呢是你可以用于记录。
- `get($key, $default)` 用于读取缓存，比如你使用 redis 缓存，那么你应该实现从 redis 读取指定 `$key` 的缓存内容返回。
- `set($key, $value, $lifetime)` 在系统需要写入缓存时调用，`$lifetime` 建议设置比 7200 小，因为微信官方的失效时间总是那么奇葩的不稳定。