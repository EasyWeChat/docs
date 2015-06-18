title: 卡券
---

本 SDK 由 `Overtrue\Wechat\Card` 提供微信卡券等服务。

### 获取实例

```php
<?php

use Overtrue\Wechat\Card;

$appId  = 'wx3cf0f39249eb0e60';
$secret = 'f1c242f4f28f735d4687abb469072a29';

$card = new Card($appId, $secret);
```


## API

+ `string create(array $base, array $properties = array(), $type = self::GENERAL_COUPON)` 创建卡券，`$base` 为微信文档上的 `$baseInfo`, `$properties` 为类型 `$type` 的其它属性；
+ `Bag get($cardId)` 卡券详情
+ `boolean update($cardId, $type, array $base = array(), array $data = array())` 更新卡券，`$base` 为微信文档上的 `$baseInfo`, `$data` 为类型 `$type` 的其它需要更新的属性；
+ `boolean delete($cardId)` 删除卡券；
+ `array lists($offset = 0, $count = 10)` 批量获取卡券列表；
+ `boolean updateStock($cardId, $amount)` 修改库存，`$amount` 用正负表示增减；
+ `boolean incStock($cardId, $amount)` 递增库存
+ `boolean decStock($cardId, $amount)` 递减库存
+ `boolean decStock($cardId, $amount)` 递减库存
+ `Bag consume($code, $cardId = null)` 卡券核销，`$code` 要消耗序列号, `$cardId` 卡券 ID。创建卡券时 use_custom_code 填写 true 时必填。非自定义 code 不必填写;
+ `Bag getCode($code, $cardId = null)`  查询 code，获取 code 的有效性(非自定义 code),该 code 对应的用户 openid、卡券有效期等信息;
+ `boolean updateCode($code, $newCode, $cardId)` 更新 code;
+ `boolean disable($code, $cardId = null)` 废弃卡券，失效，将用户的卡券设置为失效状态；
+ `string getRealCode($encryptedCode)` code 解码；
+ `array attachExtension($cardId, array $ext = array())` 返回用于批量添加到卡包的 `card_list` 项, ex:

    ```json
    {
        "card_id": "po_2Djks4-yP5PGtgGY4GkbAIIt0",
        "card_ext": "{\"code\":\"\",\"openid\":\"\",\"timestamp\":\"1402057159\",\"signature\":\"017bb17407c8e 0058a66d72dcc61632b70f511ad\"}"
    }
    ```

### 相关文档

+ [门店管理](门店管理) 创建卡券的时候需要门店
+ [颜色列表](颜色列表) 创建卡券的时候需要用到颜色列表

特殊卡券：

+ `boolean memberCardActivate($cardId, array $data)` 激活/绑定会员卡：

  商户调用接口创建会员卡获取 card_id,并将会员卡下发给用户,用户领取后需**激活/绑定** update 会员卡编号及积分信息。会员卡暂不支持转赠。

  激活/绑定会员卡支持以下两种方式:
    1. 用户点击卡券内 “bind_old_card_url”、“activate_url” 跳转商户自定义的 `H5` 页面,填写相关身份认证信息后,商户调用接口,完成激活/绑定。
    2. 商户已知用户身份或无需进行绑定等操作, 用户领取会员卡后,商户后台即时调用“激活/绑定会员卡”接口 update 会员卡编号及积分信息。

    注:code 与会员卡编号 `membership_number` 为一一对应关系。商户在调用相关查询、交易接口时,需使用 `code` 字段。

  `$data` 如下两种可选：

    ```json
    {
        "init_bonus": 100,
        "init_balance": 200,
        "membership_number": "AAA00000001", "code": "12312313",
        "card_id": "xxxx_card_id"
    }
    ```

    或

    ```json
    {
        "bonus": "www.xxxx.com",
        "balance": "www.xxxx.com",
        "membership_number": "AAA00000001",
        "code": "12312313",
        "card_id": "xxxx_card_id"
    }
    ```

+ `Bag memberCardTrade($cardId, array $data)` 会员卡交易：

  会员卡交易后每次积分及余额变更需通过接口通知微信,便于后续消息通知及其他扩展功
能。

  `$data` 如下：

    ```json
    {
        "code": "12312313",
        "card_id":"p1Pj9jr90_SQRaVqYI239Ka1erkI",
        "record_bonus": "消费30元，获得3积分",
        "add_bonus": 3,
        "add_balance": -3000
        "record_balance": "购买焦糖玛琪朵一杯，扣除金额30元。"
    }
    ```

  返回值：

    ```json
    {
        "errcode":0,
        "errmsg":"ok",
        "result_bonus": 100,
        "result_balance": 200,
        "openid":"oFS7Fjl0WsZ9AMZqrI80nbIq8xrA"
    }
    ```

+ `boolean updateMovieTicket($cardId, array $data)` 更新电影票：

  领取电影票后通过调用 “更新电影票” 接口 update 电影信息及用户选座信息。

  `$data` 如下：

    ```json
    {
        "code" : "277217129962",
        "card_id": "p1Pj9jr90_SQRaVqYI239Ka1erkI",
        "ticket_class": "4D",
        "show_time": 1408493192, "duration"：120, "screening_room": "5 号影厅",
        "seat_number": [ "5 排 14 号" , "5 排 15 号" ]
    }
    ```

+ `boolean updateMeetingTicket($cardId, array $data)` 更新会议门票：

  支持调用 “更新会议门票” 接口 update 入场时间、区域、座位等信息。

  `$data` 如下：

    ```json
    {
        "code": "717523732898",
        "card_id": "pXch-jvdwkJjY7evUFV-sGsoMl7A",
        "zone" : "C 区",
        "entrance" : "东北门",
        "seat_number" : "2 排 15 号"
    }
    ```

+ `boolean checkin($cardId, array $data)` 在线值机：

  用户点击商户指定的 URL 在线办理登机牌。办理成功 后,商户调用本接口,将值机信息同步。

  `$data` 如下：

    ```json
    {
        "code": "198374613512",
        "card_id":"p1Pj9jr90_SQRaVqYI239Ka1erkI",
        "passenger_name": "乘客姓名",
        "class": "舱等",
        "seat": "座位号",
        "etkt_bnr": "电子客票号", "qrcode_data": "二维码数据", "is_cancel ": false
    }
    ```

关于卡券接口的使用请参阅官方文档：http://mp.weixin.qq.com/wiki/9/d8a5f3b102915f30516d79b44fe665ed.html