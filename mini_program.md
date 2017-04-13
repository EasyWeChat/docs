title: 小程序 TODO
---

## 实例化

```php
<?php
use EasyWeChat\Foundation\Application;

$options = [
    // ...
    'mini_program' => [
        'app_id'   => 'component-app-id',
        'secret'   => 'component-app-secret',
        'token'    => 'component-token',
        'aes_key'  => 'component-aes-key'
        ],
    // ...
    ];

$app = new Application($options);
$miniProgram = $app->mini_program;
```

## 登录

### 通过 Code 换取 SessionKey

```php
$miniProgram->sns->getSessionKey($code);
```

## 模版消息

### 发送模版消息

> 详见[模版消息](https://easywechat.org/zh-cn/docs/notice.html)

## 小程序客服消息

### 进入事件
