# 通讯录

```php
$config = [
    'corp_id' => 'xxxxxxxxxxxxxxxxx',

    // 应用列表
    'agents' => [
        'contacts' => [
            'agent_id' => 100020,
            'secret'   => 'xxxxxxxxxx',
        ],
        //...
    ],
    //...
];

$work = Factory::work($config);

$contacts = $work->agent('contacts');
```

## 成员管理
### 创建成员

```php
$data = [
    "userid" => "overtrue",
    "name" => "超哥",
    "english_name" => "overtrue"
    "mobile" => "1818888888",
];
$contacts->user->create($data);
```

### 读取成员

```php
$contacts->user->get('overtrue');
```

### 更新成员

```php
$contacts->user->update('overtrue', [
    "isleader": 0,
    'position' => 'PHP 酱油工程师',
    //...
]);
```

### 删除成员

```php
$contacts->user->delete('overtrue');
// 或者删除多个
$contacts->user->delete(['overtrue', 'zhangsan', 'wangwu']);
```

### 获取部门成员

```php
$contacts->user->getDepartmentUsers($departmentId);
// 递归获取子部门下面的成员
$contacts->user->getDepartmentUsers($departmentId, true);
```

### 获取部门成员详情

```php
$contacts->user->getDetailedDepartmentUsers($departmentId);
// 递归获取子部门下面的成员
$contacts->user->getDetailedDepartmentUsers($departmentId, true);
```

### 用户 ID 转为 openid

```php
$contacts->user->userIdToOpenid($userId);
// 或者指定应用 ID
$contacts->user->userIdToOpenid($userId, $agentId);
```

### openid 转为用户 ID

```php
$contacts->user->openidToUserId($openid);
```

### 二次验证

企业在成员验证成功后，调用如下接口即可让成员加入成功

```php
$contacts->user->accept($userId);
```

## 部门管理

### 创建部门

```php
$contacts->department->create([
        'name' => '广州研发中心',
        'parentid' => 1,
        'order' => 1,
        'id' => 2,
    ]);
```

### 更新部门

```php
$contacts->department->update($id, [
        'name' => '广州研发中心',
        'parentid' => 1,
        'order' => 1,
    ]);
```

### 删除部门

```php
$contacts->department->delete($id);
```

### 获取部门列表

```php
$contacts->department->list();
// 获取指定部门及其下的子部门
$contacts->department->list($id);
```

## 标签管理

### 创建标签

```php
$contacts->tag->create($tagName, $tagId);
```

### 更新标签名字

```php
$contacts->tag->update($tagId, $tagName);
```

### 删除标签

```php
$contacts->tag->delete($tagId);
```

### 获取标签列表

```php
$contacts->tag->list();
```

### 获取标签成员(标签详情)

```php
$contacts->tag->get($tagId);
```

### 增加标签成员

```php
$contacts->tag->tagUsers($tagId, [$userId1, $userId2, ...]);

// 指定部门
$contacts->tag->tagDepartments($tagId, [$departmentId1, $departmentId2, ...]);
```


### 删除标签成员

```php
$contacts->tag->untagUsers($tagId, [$userId1, $userId2, ...]);

// 指定部门
$contacts->tag->untagDepartments($tagId, [$departmentId1, $departmentId2, ...]);
```




