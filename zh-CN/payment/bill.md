# 对账单

## 下载对账单

> 调用参数正确会返回一个 `EasyWeChat\Kernel\Http\StreamResponse` 对象，否则会返回相应错误信息

Example:

```php
$bill = $app->bill->download('20140603'); // type: ALL
// or
$bill = $app->bill->download('20140603', 'SUCCESS'); // type: SUCCESS

// 调用正确，`$bill` 为 csv 格式的内容，保存为文件：
$bill->saveAs('your/path/to', 'file-20140603.csv');
```

第二个参数为类型：

 - **ALL**：返回当日所有订单信息（默认值）
 - **SUCCESS**：返回当日成功支付的订单
 - **REFUND**：返回当日退款订单
 - **REVOKED**：已撤销的订单
