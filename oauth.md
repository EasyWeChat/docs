title: 网页授权
---

## 注意：`Auth` 服务不缓存 `access_token`，在获取到用户信息后请自己缓存当前用户信息，不要每次都直接请求 `$auth->authorize()`;

通常的逻辑应该是：

1. 检查你自己的 Session 中是否有登录信息，如果有则不跳授权页面
2. 如果 Session 中没有登录信息，则跳转到授权页并获取用户信息（`$auth->authorize()` 一个方法全搞定）
3. 把拿到的用户信息存到 Session 中

    - 有 SESSION -> **最终业务界面**
    - 没有 SESSION -> 用户**最终业务界面** -> **授权页** 完成授权存 SESSION -> 跳转到你的**最终业务界面**

## 友情提示

授权逻辑不要写在业务页！！！ 不然你分享出去的链接会带你自己的 `code` 出去，这样就会判断不了是否已经授权。

```php
<?php

use Overtrue\Wechat\Auth;

$appId  = 'wx3cf0f39249eb0e60';
$secret = 'f1c242f4f28f735d4687abb469072a29';

$auth = new Auth($appId, $secret);
```

### 授权并返回用户

```php
$auth->authorize($to = null, $scope = 'snsapi_userinfo', $state = 'STATE')
```

内部逻辑：

    1. 非微信回调，会自动跳转到微信授权页，并且以当前页（`$to` 为 `null` 时取当前页）作为回调页
    2. 如果已经授权返回用户对象，`$scope` 为 `snsapi_base` 时用户对象只有 `openid` 一个属性

```php

// 请一定要自己存储用户的登录信息，不要每次都授权
if (empty($_SESSION['logged_user'])) {
    $user = $auth->authorize(); // 返回用户 Bag
    $_SESSION['logged_user'] = $user;
    // 跳转到其它授权才能访问的页面
}

var_dump($_SESSION['logged_user']);
```

如果你不想用默认的逻辑，你也可以自己使用下面单独的方法来完成授权的操作：

### 其它单独使用的方法

+ 生成授权链接

  ```php
  // 生成并返回
  $auth->url($to, $scope, $state = 'STATE');
  // 直接跳转
  $auth->redirect($to, $scope, $state = 'STATE');   // 直接跳转
  ```

> 注意：
- 上面的 $scope 与 $state 通常都不用传
- `url` 与 `redirect` 参数一致
- $scope 可选为：
  + snsapi_base 只获取用户 `openid`
  + snsapi_userinfo 获取用户账号信息

example:

```php
// 只取 `openid`
$url = $auth->url('http://overtrue.me', 'snsapi_base');
// 需要拉取用户信息
$url = $auth->url('http://overtrue.me', 'snsapi_userinfo');
```

+ 获取已授权用户

  ```php
  $auth->user();
  ```

+ 获取授权许可信息

  在每一次使用 code 得到 access_token 后，以下信息会存储：

  ```json
  {
     "access_token":"ACCESS_TOKEN",
     "expires_in":7200,
     "refresh_token":"REFRESH_TOKEN",
     "openid":"OPENID",
     "scope":"SCOPE",
     "unionid": "o6_bmasdasdsad6_2sgVt7hMZOPfL"
  }
  ```

  你可以用以下方式获取，用来存储到你的数据库个人信息中

  ```php
  $auth->access_token;// 获取本次授权后的 access_token
  $auth->refresh_token;// 获取本次授权后的 refresh_token
  // ...
  ```

+ 检查 access_token 是否有效

  ```php
  $auth->accessTokenIsValid($accessToken, $openid); // 返回 boolean
  ```

+ 使用 refresh_token 刷新 access_token

  ```php
  $auth->refresh($refreshToken); // 注意这里不是access_token
  ```
+ 使用 access_token 获取用户信息

  ```php
  $auth->getUser($openId, $accessToken);
  ```

更多关于微信网页授权 API 请参考： http://mp.weixin.qq.com/wiki/17/c0f37d5704f0b64713d5d2c37b468d75.html