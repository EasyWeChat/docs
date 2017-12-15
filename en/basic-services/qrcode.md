# QR Code

There are 2 types of QR code：

1. Temporary QR Code， there is expired time。the maximum expiration time can be set to **30 days**，but it can generate many。temporary QR Code is mainly used for account binding and other business scenarios that do not require QR Code save forever。
2. Permanent Qr Code，no expiration time，but a small number（currently up to 100,000）。Permanent Qr Code is mainly used for account binding。user source statistics and other scenarios。

## Get  Instance

```php
<?php
use EasyWeChat\Foundation\Application;

// ...

$app = new Application($options);

$qrcode = $app->qrcode;
```


## API

+ `Bag temporary($sceneId, $expireSeconds = null)` Create  Temporary QR Code；
+ `Bag forever($sceneValue)` Create Permanent Qr Code
+ `Bag card(array $card)` Create Card and  Coupons Qr Code
+ `string url($ticket)` Get the QR code URL，usage： `<img src="<?php $qrcode->url($qrTicket); ?>">`；

### Create  Temporary QR Code

```php
$result = $qrcode->temporary(56, 6 * 24 * 3600);

$ticket = $result->ticket;// or $result['ticket']
$expireSeconds = $result->expire_seconds; // effective seconds
$url = $result->url; // Qr Code image analysis of the address, developers can generate the required Qr Code pictures based on the address
```

### Create Permanent Qr Code

```php
$result = $qrcode->forever(56);// or $qrcode->forever("foo");

$ticket = $result->ticket; // or $result['ticket']
$url = $result->url;
```

### Get the QR code URL

```php
$url = $qrcode->url($ticket);
```

### Create Card and  Coupons Qr Code

```php
$qrcode->card($card);
```

### Get Qr Code contents

```php
$url = $qrcode->url($ticket);

$content = file_get_contents($url); // get binary image content

file_put_contents(__DIR__ . '/code.jpg', $content); // write the file
```
