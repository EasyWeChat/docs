# 自定义菜单

自定义菜单是指为单个应用设置自定义菜单功能，所以在使用时请注意调用正确的应用实例。

```php
$app = Factory::work($config);

$menu = $app->menu;
```

## 创建菜单

```php
$menus = [
    'button' => [
        [
            'name' => '首页',
            'type' => 'view',
            'url' => 'https://easywechat.com'
        ],
        [
            'name' => '关于我们',
            'type' => 'view',
            'url' => 'https://easywechat.com/about'
        ],
        //...
    ],
];

$menu->create($menus);
```

## 获取菜单

```php
$menu->get();
```

## 删除菜单

```php
$menu->delete();
```