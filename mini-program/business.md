# 小程序客服子商户能力

## 功能介绍
客服子商户能力，是微信公众平台为综合服务平台型小程序提供的客服能力支持。
一个小程序帐号可为平台内的商户创建多个子商户帐号，创建后在小程序客服组件唤起子商户单独的会话。多个子
商户会话独立，可为用户提供更优质的客服体验。

## 开放范围
对【电商平台】类目小程序开放。  
该功能不支持服务商第三方平台

API:

#### 获取实例

```php
$service = $app->business;
```

#### 创建商户
`头像，图片类型，需要用临时素材接口得
到：https://mp.weixin.qq.com/debug/wxadoc/dev/api/custommsg/material.html#新
增临时素材`

```php
$app->business->register('overtrue', '小超', 'media_id');
```

#### 更新商户信息

```php
$app->business->update($businessId, '小小超');
```

#### 拉取单个商户信息

```php
$app->business->getBusiness($businessId);
```

#### 拉取多个商户信息

```php
$app->business->list(0, 10);
```

#### 消息推送

```php
// 可以发送文本，图片，小程序卡片等  可参考: https://www.easywechat.com/docs/5.x/official-account/messages
//小程序卡片
use EasyWeChat\Kernel\Messages\MiniProgramPage;

$msg = new MiniProgramPage([
    'title' => 'overtrue',
    'appid' => 'my-app-id',
    'pagepath' => 'pages/home/index',
    'thumb_media_id' => 'media-id',
]);

// 文本
use EasyWeChat\Kernel\Messages\Text;

$msg = new Text('您好！overtrue。');

// 图片
use EasyWeChat\Kernel\Messages\Image;

$msg = new Image($mediaId);

// 发送
$app->business->message($msg)->business($businessId)->to('open-id')->send();
```

#### 客服输入状态

```php
$app->business->typing($businessId, 'open-id');
```