title: 错误处理
---

所有的错误均使用异常抛出，异常类名为 `EasyWeChat\Core\Exception`, 你可以使用常规的异常处理方法来处理本 SDK 所有的异常：

```php
$app->setExceptionHandler(function($exception){
  error_log("code:" . $exception->getCode . ' exception: ' . $exception->getMessage());
});
```

这里的回调函数的第一个参数 `$exception` 为继承自 [`Exception`](http://php.net/manual/zh/class.exception.php) 的异常类，所以你可以使用 `Exception` 类的所有方法。

- `$error->getCode()` 错误码，ex: `40001`
- `$error->getMessage()` 错误消息
- `$error->getLine()` 错误行

[PHP 错误处理](http://php.net/manual/zh/book.errorfunc.php)
[PHP 异常类](http://php.net/manual/zh/class.exception.php)

当请求微信服务器失败或者返回错误时同样会触发此逻辑，错误代码请参考：[微信官方文档](http://mp.weixin.qq.com/wiki/) 中**全局错误码章节**