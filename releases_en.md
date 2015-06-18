title: Release Notes.
---

# 2.1
## [2.1.0-alpha](https://github.com/overtrue/wechat/tree/2.1.0-alpha)
- [MOD] Re-structure all of the SDK arguments from plain passed as array passed.
- :broken_heart: It doesn't back compatible of the 2.0 serial.

# 2.0
## [2.0.20](https://github.com/overtrue/wechat/tree/2.0.20)

- [FIX] `Overtrue\Wechat\Notice::addTemplate`, typo of the RPC address

## [2.0.19](https://github.com/overtrue/wechat/tree/2.0.19)

- [FIX] `Overtrue\Wechat\Staff::delete`, let it works;

## [2.0.18](https://github.com/overtrue/wechat/tree/2.0.18)

- [MOD] `Overtrue\Wechat\Menu`, `view_limited` and `media_id` events supported;

## [2.0.0-alpha](https://github.com/overtrue/wechat/tree/2.0.0-alpha)

- [DEL] Removed `Overtrue\Wechat\Wechat` class;
- [DEL] Removed `Menu::make()` method，the usage is changed to `new MenuItem($name, $type, $key)` style;
- [DEL] Relocated `Auth::authorized()` method;
- [NEW] JSSDK supported;
- [NEW] QRCode supported;
- [NEW] Map to the Exception message as Chinese;
- [NEW] Store supported;
- [NEW] Template Message Service;
- [NEW] WXCard color properties;
- [NEW] Translation Service;
- [NEW] Statistics Service;
- [NEW] multimedia resorce management service;
- [MOD] Changed the picutures auto-uploading behavior, the `media($path)` is only used pre-uploaded resource `media_id` for now;
- [MOD] Removed `Wechat::service('cache')` static method, relocated usage such as `$userService = new Overtrue\Wechat\User($appId, $secret)` style;
- [MOD] Removed all of the `->all()` methods, Relocated the usages as `->lists()`;
