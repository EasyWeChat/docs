# 小程序码

## 获取小程序码

### 接口A: 适用于需要的码数量较少的业务场景

API:

```
$miniProgram->app_code->get(string $path, int $width, bool $autoColor, array $lineColor);
```

> 其中 $path 必填，其余参数可留空。
>

示例代码：

```php
$response = $miniProgram->app_code->get('path/to/page');

// 保存小程序码到文件
file_put_contents('/path/to/appcode.png', $response);
```

### 接口B：适用于需要的码数量极多，或仅临时使用的业务场景

API:

```
$miniProgram->app_code->getUnlimit(string $scene, string $page = null, int $width = null, bool $autoColor = null, array $lineColor = null);
```

> 其中 $scene 必填，其余参数可留空。

示例代码：

```php
$response = $miniProgram->app_code->getUnlimit('scene-value');

// 保存小程序码到文件
file_put_contents('/path/to/appcode.png', $response);
```

## 获取小程序二维码

API:

```
$miniProgram->app_code->qrcode(string $path, int $width = null);
```

> 其中 $path 必填，其余参数可留空。

示例代码：

```php
$response = $miniProgram->app_code->qrcode('/path/to/page');

// 保存小程序码到文件
file_put_contents('/path/to/appcode.png', $response);
```

## 
