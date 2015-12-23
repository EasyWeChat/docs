title: 多客服消息转发
---


多客服的消息转发绝对是超级的简单，转发的消息类型为 `transfer`：

 ```php

  use Overtrue\Wechat\Message;

  // 转发收到的消息给客服
  $server->on('message', function($message) {
      return Message::make('transfer');
  });

  $result = $server->serve();

  echo $result;
  ```

当然，你也可以指定转发给某一个客服：

```php
return Message::make('transfer')->account($account);
// 或者
return Message::make('transfer')->to($account);
```

更多关于多客服消息转发：http://mp.weixin.qq.com/wiki/5/ae230189c9bd07a6b221f48619aeef35.html