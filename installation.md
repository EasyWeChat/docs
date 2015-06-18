title: 安装
---

## 环境要求
- PHP >= 5.3.0
- [PHP cURL 扩展](http://php.net/manual/en/book.curl.php)
- [PHP Mcrypt 扩展](http://php.net/manual/en/book.mcrypt.php) 当启用加密消息时必选


[Laravel 5 拓展包: overtrue/laravel-wechat](https://github.com/overtrue/laravel-wechat)

## 安装

1. 使用 [composer](http://getcomposer.org/):

  ```shell
  composer require overtrue/wechat
  ```

2. 手动安装

  - 下载 [最新版zip包](https://github.com/overtrue/wechat/archive/master.zip)
  - 或者下载 [指定版本](https://github.com/overtrue/wechat/releases)

  然后引入根目录的 `autoload.php` 即可：

  ```php
  <?php

  require __DIR__ . "/wechat/autoload.php"; // 路径请修改为你具体的实际路径
  ...
  ```