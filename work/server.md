# 服务端

企业微信服务端推送和公众号一样，请参考：[公众号：服务端](/docs/{{version}}/official-account/server.md)

## 第三方平台推送事件

企业微信数据推送的有以下事件：

- 通讯录变更 `change_contact`
- 批量任务执行完成 `batch_job_result`


## 自定义消息处理器

> *消息处理器详细说明见：公众号开发 - 服务端一节*

```php
// 处理通讯录变更事件
$server->handleContactChanged(callable | string $handler);

// 处理任务执行完成事件
$server->handleBatchJobCompleted(callable | string $handler);
```
