title: JSSDK
---

本 SDK 同样由 `Overtrue\Wechat\Js` 提供了 JSSDK 相关的功能。

### 获取实例

```php
<?php

use Overtrue\Wechat\Js;

$appId  = 'wx3cf0f39249eb0e60';
$secret = 'f1c242f4f28f735d4687abb469072a29';

$js = new Js($appId, $secret);
```
### API

- `$js->config(array $APIs, $debug = false, $json = true);` 获取JSSDK的配置数组，默认返回 JSON 字符串，当 `$json` 为 `false` 时返回数组，你可以直接使用到网页中。
- `$js->setUrl($url)` 设置当前URL，如果不想用默认读取的URL，可以使用此方法手动设置，通常不需要。

example:

我们可以生成js配置文件：

```js
<script src="http://res.wx.qq.com/open/js/jweixin-1.0.0.js" type="text/javascript" charset="utf-8"></script>
<script type="text/javascript" charset="utf-8">
    wx.config(<?php echo $js->config(array('onMenuShareQQ', 'onMenuShareWeibo'), true, true) ?>);
</script>
```
结果如下：

```js
<script src="http://res.wx.qq.com/open/js/jweixin-1.0.0.js" type="text/javascript" charset="utf-8"></script>
<script type="text/javascript" charset="utf-8">
wx.config({
    debug: true,
    appId: 'wx3cf0f39249eb0e60',
    timestamp: 1430009304,
    nonceStr: 'qey94m021ik',
    signature: '4F76593A4245644FAE4E1BC940F6422A0C3EC03E',
    jsApiList: ['onMenuShareQQ', 'onMenuShareWeibo']
});
</script>
```