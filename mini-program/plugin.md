# 插件管理

## 申请使用插件

```php
$pluginAppId = 'xxxxxxxxx';

$app->plugin->apply($pluginAppId);
```

## 删除已添加的插件

```php
$pluginAppId = 'xxxxxxxxx';

$app->plugin->unbind($pluginAppId);
```

## 查询已添加的插件

```php
$app->plugin->list();
```
