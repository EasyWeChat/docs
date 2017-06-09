title: 留言管理
---

# Unreleased

## 获取实例

```php
<?php
use EasyWeChat\Foundation\Application;

$app = new Application($options);

$comment = $app->comment;
```

## API 列表

### 打开已群发文章评论

```php
$comment->open($dataId, $index = null);
```

### 关闭已群发文章评论

```php
$comment->close($dataId, $index = null);
```

### 查看指定文章的评论数据

```php
$comment->lists($dataId, $index, $begin, $count, $type);
```

### 将评论标记精选

```php
$comment->markElect($dataId, $index, $commentId);
```

### 将评论取消精选

```php
$comment->unmarkElect($dataId, $index, $commentId);
```

### 删除评论

```php
$comment->delete($dataId, $index, $commentId);
```

### 回复评论

```php
$comment->reply($dataId, $index, $commentId, '回复内容');
```

### 删除评论回复

```php
$comment->deleteReply($dataId, $index, $commentId);
```
