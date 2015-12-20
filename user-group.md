title: 用户组
---

## 用户组

```php
<?php

use Overtrue\Wechat\Group;

$appId  = 'wx3cf0f39249eb0e60';
$secret = 'f1c242f4f28f735d4687abb469072a29';

$group = new Group($appId, $secret);
```

+ `$group->lists();` 获取所有分组
+ `$group->create($name);` 创建分组
+ `$group->update($groupId, $name);` 修改分组信息
+ `$group->delete($groupId);` 删除分组
+ `$group->moveUser($openId, $groupId);` 移动单个用户到指定分组
+ `$group->moveUsers(array $openIds, $groupId);` 批量移动用户到指定分组