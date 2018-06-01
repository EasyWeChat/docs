# Content Security interfaces

## Text security check

Used to check whether a text contains illegal content.

### Frequency limit

At most 2000 times/min, 1,000,000 times/day for each unique appid.

### Example

```php
// pass the text taht what to check as the parameter, the content should be NOT longer than 500K Bytes.
$content = '你好';

$result = $app->content_security->checkText($content);

// normally, it returns 0
{
    "errcode": "0",
    "errmsg": "ok"
}

// when $content contains risky content, it returns 87014
{
    "errcode": 87014,
    "errmsg": "risky content"
}
```

## Image security check

Used to check whether an iamge contains risky content. Such as porn, involving sensitive human faces (usually political figures).

### Frequency limit

At most 1000 times/min, 100,000 times/day for each unique appid.

### Example

```php
// the parameter refers to the real path of the image, support PNG/JPE/JPG/GIF, and it should be  smaller than 750 x 1334(pixels size), and smaller than 300K (file size)
$result = $app->content_security->checkImage('/path/to/the/image');

// normally, it returns 0
{
    "errcode": "0",
    "errmsg": "ok"
}

// When the image contains risky content, it returns 87014
{
    "errcode": 87014,
    "errmsg": "risky content"
}
```

## Example

The TWO interfaces above can only be used in mini-programe so far, the variable `$app` refers to the mini-program instance.

```php
use EasyWeChat\Factory;

$config = [
    'app_id' => 'wx3cf0f39249eb0exx',
    'secret' => 'f1c242f4f28f735d4687abb469072axx',

    'response_type' => 'array',

    'log' => [
        'level' => 'debug',
        'file' => __DIR__.'/wechat.log',
    ],
];

$app = Factory::miniProgram($config);
```
