# 小程序码

## 获取小程序码

### 接口A: 适用于需要的码数量较少的业务场景

API:

```
$miniProgram->app_code->get(string $path, array $optional = []);
```

> 其中 $optional 为以下可选参数：
> width Int - 默认 430 二维码的宽度
> auto_color  默认 false  自动配置线条颜色，如果颜色依然是黑色，则说明不建议配置主色调
> line_color  示例：{"r":"0","g":"0","b":"0"} auth_color 为 false 时生效，使用 rgb 设置颜色 例如 {"r":"xxx","g":"xxx","b":"xxx"}

示例代码：

```php
$response = $miniProgram->app_code->get('path/to/page');
// 或者
$response = $miniProgram->app_code->get('path/to/page', [
    'width' => 600,
    //...
]);

// $response 为 EasyWeChat\Kernel\Http\StreamResponse 实例

// 保存小程序码到文件
$filename = $response->saveAs('/path/to/directory');
// 或
$filename = $response->saveAs('/path/to/directory', 'appcode.png');
```

### 接口B：适用于需要的码数量极多，或仅临时使用的业务场景

API:

```
$miniProgram->app_code->getUnlimit(string $scene, array $optional = []);
```

> 其中 $scene 必填，$optinal 与 get 方法一致，多一个 page 参数。

示例代码：

```php
$response = $miniProgram->app_code->getUnlimit('scene-value', [
    //...
]);

// $response 为 EasyWeChat\Kernel\Http\StreamResponse 实例

// 保存小程序码到文件
$filename = $response->saveAs('/path/to/directory');
// 或
$filename = $response->saveAs('/path/to/directory', 'appcode.png');
```

## 获取小程序二维码

API:

```
$miniProgram->app_code->getQrCode(string $path, int $width = null);
```

> 其中 $path 必填，其余参数可留空。

示例代码：

```php
$response = $miniProgram->app_code->qrcode('/path/to/page');

// $response 为 EasyWeChat\Kernel\Http\StreamResponse 实例

// 保存小程序码到文件
$filename = $response->saveAs('/path/to/directory');
// 或
$filename = $response->saveAs('/path/to/directory', 'appcode.png');
```

##
