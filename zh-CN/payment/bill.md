# 对账单

## 下载对账单

```php
$bill = $app->bill->download(20140603); // type: ALL
// or
$bill = $app->bill->download('20140603', 'SUCCESS'); // type: SUCCESS
// bill 为 csv 格式的内容

// 保存为文件
file_put_contents('YOUR/PATH/TO/bill-20140603.csv', $bill);

// 如发生错误（比如日期不正确），会返回 json 错误信息
```

第二个参数为类型：

 - **ALL**：返回当日所有订单信息（默认值）
 - **SUCCESS**：返回当日成功支付的订单
 - **REFUND**：返回当日退款订单
 - **REVOKED**：已撤销的订单
