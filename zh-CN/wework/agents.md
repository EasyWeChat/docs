# 应用管理

应用管理是企业微信中比较特别的地方，因为它的使用是不基于应用的，或者说基于任何一个应用都能访问这些 API，所以在用法上是直接调用 work 实例的 `agent` 属性。

```php
$config = [
    ...
];

$work = Factory::work($config);
```

## 应用列表

```php
$agents = $work->agent->list();
```

## 应用详情

```php
$agents = $work->agent->get($agentId);
```

## 设置应用

```php
$agents = $work->agent->set($agentId, ['foo' => 'bar']);
```