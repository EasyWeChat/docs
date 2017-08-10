# 用户

用户信息的获取是微信开发中比较常用的一个功能了，以下所有的用户信息的获取与更新，都是**基于微信的 `openid` 的，并且是已关注当前账号的**，其它情况可能无法正常使用。

## 获取用户信息

获取单个：

```php
$user = $app->user->get($openId);
```

获取多个：

```php
$users = $app->user->select([$openId1, $openId2, ...]);
```

## 获取用户列表

```php
$app->user->list($nextOpenId = null);  // $nextOpenId 可选
```

示例：

```php
 $users = $app->user->list();

// result
 {
  "total": 2,
  "count": 2,
  "data": {
    "openid": [
      "OPENID1",
      "OPENID2"
    ]
  },
  "next_openid": "NEXT_OPENID"
}
```

## 修改用户备注

```php
$app->user->remark($openId, $remark); // 成功返回boolean
```

示例：

```php
$app->user->remark($openId, "僵尸粉");
```

## 拉黑用户

```php
$app->user->block('openidxxxxx');
// 或者多个用户
$app->user->block(['openid1', 'openid2', 'openid3', ...]);
```

## 取消拉黑用户

```php
$app->user->unblock('openidxxxxx');
// 或者多个用户
$app->user->unblock(['openid1', 'openid2', 'openid3', ...]);
```

## 获取黑名单

```php
$app->user->blacklist($beginOpenid = null); // $beginOpenid 可选
```