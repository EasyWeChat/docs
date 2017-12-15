# JSSDK

```php
<?php
use EasyWeChat\Foundation\Application;
//...
$app = new Application($options);

$js = $app->js;
```

## API

- `$js->config(array $APIs, $debug = false, $beta = false, $json = true);` 

Get `JSSDK` configuration array ，default returns a JSON string，if `$json` is `false` then return an array，You can use it directly into the webpage。

- `$js->setUrl($url);`

Set the current `URL`, if you do not want to use the default URL to read, you can use this method manually set, usually do not need。

Example:

We can generate js configuration file：

```js
<script src="http://res.wx.qq.com/open/js/jweixin-1.0.0.js" type="text/javascript" charset="utf-8"></script>
<script type="text/javascript" charset="utf-8">
    wx.config(<?php echo $js->config(array('onMenuShareQQ', 'onMenuShareWeibo'), true) ?>);
</script>
```
The above code will produce the following result：

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

More use please refer to [WeChat official document](http://mp.weixin.qq.com/wiki/) in the "JSSDK" chapter

更多 JSSDK 的使用请参考  中 **JSSDK章节**