# 服务端

企业微信第三方服务端推送和公众号一样，请参考：[公众号：服务端](/docs/{{version}}/official-account/server.md)

## 第三方平台推送事件

企业微信第三方数据推送的有以下事件：

- suite_ticket 推送 `suite_ticket`
- 授权成功 `create_auth`
- 授权变更 `change_auth`
- 授权取消 `cancel_auth`
- 通讯录变更 `change_contact`
- 共享应用事件回调 `share_agent_change`

## 自定义消息处理器

> *消息处理器详细说明见：公众号开发 - 服务端一节*

```php
// 授权成功事件
$server->handleAuthCreated(callable | string $handler);

// 授权变更事件
$server->handleAuthChanged(callable | string $handler);

// 授权取消事件
$server->handleAuthCancelled(callable | string $handler);

// 通讯录变更事件
$server->handleContactChanged(callable | string $handler);

// 共享应用事件
$server->handleShareAgentChanged(callable | string $handler);

// suite_ticket 推送事件
$server->handleSuiteTicketRefreshed(callable | string $handler);
```

> {warning} 注意：如果你自行处理了 SuiteTicket 推送，你必须同时设置 ProviderAccessToken 类，因为 ProviderAccessToken 依赖它。
