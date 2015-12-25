title: 升级日志
---

# 2.1

- 新增支付
- 新增红包
- 新增群发

# 2.0
## [2.0.20](https://github.com/overtrue/wechat/tree/2.0.20)

- 【Bug】模板消息API地址错误

## [2.0.19](https://github.com/overtrue/wechat/tree/2.0.19)

- 【Bug】更新删除客服

## [2.0.18](https://github.com/overtrue/wechat/tree/2.0.18)

- 【更新】自定义菜单添加两种类型

## [2.0.0-alpha](https://github.com/overtrue/wechat/tree/2.0.0-alpha)

- 【删除】移除 `Overtrue\Wechat\Wechat` 基类，便于处理多用户，各业务分散到独立的类中完成；
- 【删除】移除 `Menu::make()` 方法，改为 `new MenuItem($name, $type, $key)` 形式；
- 【删除】移动 `Auth::authorized()` 方法；
- 【新增】JSSDK 服务；
- 【新增】二维码服务；
- 【新增】异常消息添加中文错误消息；
- 【新增】门店服务；
- 【新增】模板消息；
- 【新增】卡券颜色列表；
- 【新增】语义理解服务；
- 【新增】数据统计查询服务；
- 【新增】素材添加完整 API 支持，支持永久素材等官方提供的所有功能；
- 【变更】消息中图片等附件不再自动上传，之前 `media($path)` 不再接受本地文件路径，只接受上传后的 `media_id` 值，上传请先使用素材管理服务进行上传，便于素材管理；
- 【变更】 `Wechat::service('cache')` 同样不存在了，更改为 new 服务(...), ex: `$userService = new Overtrue\Wechat\User($appId, $secret)`；
- 【变更】 之前服务里的 `->all()` 方法均改名为 `->lists()`；
