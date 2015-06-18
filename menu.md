title: 自定义菜单
---

本 SDK 由 `Overtrue\Wechat\Menu` 与 `Overtrue\Wechat\MenuItem` 提供微信菜单的管理服务。

自定义菜单功能的使用场景：你自己做了网站后台，想要管理某个公众号的菜单，在后台填写表单后，经由 SDK 请求微信的服务器完成菜单设定。

## 概念

#### 菜单项 `MenuItem`

菜单的组成单位，分为一级与二级，一个微信菜单包含最多3个一级菜单项。一个一级菜单项可以包含最多5个二级菜单项。


## 注意事项

目前自定义菜单最多包括3个一级菜单，每个一级菜单最多包含5个二级菜单。一级菜单最多4个汉字，二级菜单最多7个汉字，多出来的部分将会以“...”代替。请注意，创建自定义菜单后，由于微信客户端缓存，需要24小时微信客户端才会展现出来。建议测试时可以尝试取消关注公众账号后再次关注，则可以看到创建后的效果。

### 获取实例

```php
<?php

use Overtrue\Wechat\Menu;
use Overtrue\Wechat\MenuItem;

$appId  = 'wx3cf0f39249eb0e60';
$secret = 'f1c242f4f28f735d4687abb469072a29';

$menu = new Menu($appId, $secret);
```


## API

+ `array $menu->get();` 读取菜单
+ `boolean $menu->set(array $menus);` 设置菜单，参数为一个包含最多三个一级菜单项的数组
+ `boolean $menu->delete();` 删除菜单
+ `new MenuItem($name, $type = null, $key = null)` 创建一个菜单项
  - `$name` 菜单项名称，比如：`今日歌曲`
  - `$type` 菜单项类型，比如：`view`,`click`等，更多请参考 http://mp.weixin.qq.com/wiki `自定义菜单` 章节。
  - `$key`  菜单项的值，当 `$type` 为 `view` 时为目标 URL，其它为自定义 key。

一个菜单项可以使用 `buttons` 方法传入一个菜单项数组创建二级菜单项:

``

> 注意：由于微信客户端缓存，需要24小时微信客户端才会展现出来。建议测试时可以尝试取消关注公众账号后再次关注，则可以看到创建后的效果。

example:

```php

$button = new MenuItem("菜单");

$menus = array(
            new MenuItem("今日歌曲", 'click', 'V1001_TODAY_MUSIC'),
            $button->buttons(array(
                    new MenuItem('搜索', 'view', 'http://www.soso.com/'),
                    new MenuItem('视频', 'view', 'http://v.qq.com/'),
                    new MenuItem('赞一下我们', 'click', 'V1001_GOOD'),
                )),
        );

try {
  $menu->set($menus);// 请求微信服务器
  echo '设置成功！';
} catch (\Exception $e) {
  echo '设置失败：' . $e->getMessage();
}

```

生成结果：

```json
 {
     "button":[
     {
          "type":"click",
          "name":"今日歌曲",
          "key":"V1001_TODAY_MUSIC"
      },
      {
           "name":"菜单",
           "sub_button":[
           {
               "type":"view",
               "name":"搜索",
               "url":"http://www.soso.com/"
            },
            {
               "type":"view",
               "name":"视频",
               "url":"http://v.qq.com/"
            },
            {
               "type":"click",
               "name":"赞一下我们",
               "key":"V1001_GOOD"
            }]
       }]
 }
```

### 创建子菜单

`Overtrue\Wechat\MenuItem` 对象有一个方法 `buttons(array $items)` 指定**二级菜单**(子菜单)。

```php
$button = new MenuItem('菜单');
$button->buttons(array(
            new MenuItem('搜索', 'view', 'http://www.soso.com/'),
            new MenuItem('视频', 'view', 'http://v.qq.com/'),
            new MenuItem('赞一下我们', 'click', 'V1001_GOOD'),
        ));

```

以上就会构成以下样子的菜单：

```
菜单
    搜索
    视频
    赞一下我们
```

## Laravel 示例

输入菜单数组（从你的网站后台表单创建）：
```json
[
    {
        "name":"博客",
        "type":"view",
        "key" :"http://overtrue.me"
    },
    {
        "name": "更多",
        "type": null,
        "key": null,
        "buttons":[
            {
                "name":"GitHub",
                "type":"view",
                "key" :"https://github.com/overtrue"
            },
            {
                "name":"微博",
                "type":"view",
                "key" :"http://weibo.com/44294631"
            }
        ]
    }
]
```

控制器示例：

```php
<?php

use Overtrue\Wechat\Menu;
use Overtrue\Wechat\MenuItem;

class WechatController {
     //...
    public function setWechatMenu()
    {
        $appId  = 'wx3cf0f39249eb0e60';
        $secret = 'f1c242f4f28f735d4687abb469072a29';

        $menu = new Menu($appId, $secret);

        $menus = Input::get('menus'); // menus 是你自己后台管理中心表单post过来的一个数组

        $target = [];

        // 构建你的菜单
        foreach ($menus as $menu) {
            // 创建一个菜单项
            $item = new MenuItem($menu['name'], $menu['type'], $menu['key']);

            // 子菜单
            if (!empty($menu['buttons'])) {
                $buttons = [];
                foreach ($menu['buttons'] as $button) {
                    $buttons[] = new MenuItem($button['name'], $button['type'], $button['key']);
                }

                $item->buttons($buttons);
            }

            $target[] = $item;
        }

        $menu->set($target); // 失败会抛出异常

        return Redirect::back()->withMessage('菜单设置成功！');
    }
     //...
}

```

更多关于微信自定义菜单 API 请参考： http://mp.weixin.qq.com/wiki `自定义菜单` 章节。