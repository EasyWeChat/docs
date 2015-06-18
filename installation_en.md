title: Installations
---

## Environments

- PHP >= 5.3.0
- [PHP cURL extension](http://php.net/manual/en/book.curl.php)
- [PHP Mcrypt extension](http://php.net/manual/en/book.mcrypt.php) optional required while enable the encrypt transfer feature

[Laravel 5 SDK wrapper](https://github.com/overtrue/laravel-wechat)

## Installations
1. By [composer](http://getcomposer.org/):

  ```shell
  composer require overtrue/wechat
  ```

2. By Downlads

 - [download the lastest version zip package](https://github.com/overtrue/wechat/archive/master.zip)
 - [or the archived version](https://github.com/overtrue/wechat/releases)

 and then point-out the package `autoload.php` file such as:

  ```php
  <?php

  require __DIR__ . "/wechat/autoload.php";
  ...
  ```
