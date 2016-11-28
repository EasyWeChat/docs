title: Troubleshooting
---

### Troubleshooting

We gathered some most frequently met problems and their solutions. Some may fit your speific situations, while some will not. If you found any of these answers not working for your occasion, please open an issue.

We do hope this page could offer you the help you need, and narrow the gap of development for WeChat, to make it even enjoyable.

# Basic server problems

- Incorrect timezone: run `date` command on your server to check current time. For modifying the timezone, please refer to [Setting The Correct Timezone In CentOS And Ubuntu Servers With NTP](https://www.liberiangeek.net/2013/02/setting-the-correct-timezone-in-centos-and-ubuntu-servers-with-ntp/).
    - ...

## curl: (60) SSL certificate problem: unable to get local issuer certificate

This is the problem with SSL certificates. While working with WeChat Payment functionalities you may see the error "SSL certificate problem: unable to get local issuer certificate".

WeChat Public Platform suggests that developers should use HTTPS to access the API, e.g. WeChat Payment, or the [Red Packet](http://blog.wechat.com/2016/01/27/we-chat-about-wechat-5-red-packets-wechats-secret-weapon-in-payments/) that involves financial assets operations. WeChat SDK follows this suggestion, so please make sure your server has the proper CA certificates configured, and make changes to your code according to the official documentations.

1. Download the CA certificate

  You can use the certificates from http://curl.haxx.se/ca/cacert.pem, or the [WeChat official site](https://pay.weixin.qq.com/wiki/doc/api/app.php?chapter=4_3) (look for the `rootca.pem` download).

2. Configure your CA certificate in `php.ini`

  Save the aforementioned CA file to your server, then open `php.ini` and find `curl.cainfo`, change the value to the **absolute path** to the CA file. Save `php.ini` and restart `php-fpm`.

  ```
  curl.cainfo = /path/to/downloaded/cacert.pem
  ```
  > Note: use **absolute path** which suits your own situation.

  It is now allowed to modify the source files in HTTP class in other ways.

## cURL error 56: SSLRead() return error -9806

This is due to some issues with the homebrew version of PHP 7.0 on macOS. The solution is simply reinstall PHP with brew.

```shell
$ brew install homebrew/php/php70 --with-homebrew-openssl --with-homebrew-curl --without-snmp -vvv
```

Check:

```shell
$ php -i | grep 'OpenSSL support'

OpenSSL support => enabled
OpenSSL support => enabled
```


## Payment failed! Current URL is not registered (支付失败！当前页面的 URL 未注册)

This is a common mistake on the developer side. You need to configure the authorized directory in WeChat Plubic Platform. The options are located in **微信支付 -> 开发设置 (WeChat Payment -> Development Config)**.

Note: 

1. WeChat supports 3 authorized directories at most, which may be helpful for your multiple apps based on a single official account.
2. The correct format for **【支付授权目录】(authorized directory)** must start with `http://` or `https://`, and end with a slash `/`. Plus, the domain name must have an ICP license number.
3. The authorized directory should contain a **two-level or three-level directory** if needed.
4. All pages that actually send payment requests **must be under the path of your authorized directories**.
5. Additionally, if you need to test your payment functionality, you need to add your test WeChat account into the whitelist. Otherwise you may encounter some errors.

> You may want to learn some basic terms about web app development beforehand, e.g. **page**, **directory**, **URL** and **domain names** etc. Skip this info if you're experienced. :smile:

## Redirect_url params error (redirect_url 参数错误)

This is due to your app used **web authorization** without properly configuring the **网页授权域名 (web authorization domain name)**. Please login to [WeChat Public Platform](https://mp.weixin.qq.com/), and find the **网页授权获取用户基本信息 (Web Authorization to obtain basic user information)** options under **开发 -> 接口权限 (Development -> API Permissions)**.

1. The domain name must have an ICP license number. Or it could not be saved.
2. The **web authorization domain name** means the url to redirect to after you got the authorization **code**. In most occasions, it is your business domain name.
3. It will take effect immediately after you have saved the domain name.
4. WeChat supports only 1 web authorization domain name. So you may need some extra work planning your web app.

## [JSAPI] config: invalid url domain

This is related to the JSAPI parameter in the `wx.config()` method of the JS-SDK. You'll see this error when **JSAPI 安全域名 (Safe Domain Names)** is not correctly configured, AND debug mode on.

The solution is go to WeChat Public Platform, and add your domain name to the list of **JSAPI 安全域名 (JSAPI Safe Domain Names)** in **公众号设置 -> 功能设置 (Official Account Options -> Functionality)**.

1. WeChat supports 3 safe domain names at most, all of which must have ICP license numbers. They can be top level or any lower level domains.
2. You can edit the JSAPI Safe Domain Names 3 times per month. Please be cautious.
3. If you need to call the Payment API by JSAPI on some web page, the URL must be under the **Safe Domain Names** list, while still in the **Payment Authorization Directories** list.

## Token validation failure / No response after sending messages to your official account

The most usual two types of mistakes are as follows:

1. Please make sure you have **enabled** the development mode. Even with a validated token, doesn't mean it's enabled. You can tell that the development mode is actually enabled by seeing the red **停用 (Disable)**.
2. If you see **Token 验证失败 (Failed to validate token)** when clicking the Save button, you'll need to find out from all basic causes, e.g. server URL / token not configured, or network issues.
3. If no response given from your official account after you've sent messages to it, even the logs on your server is white as snow about those messages, please try disable and enable the **服务器配置 (Server Config)** multiple times. This is a gambling game.
4. Use the [online debug tool](http://mp.weixin.qq.com/debug/). When you see the green **请求成功 (Request Succeeded)** then your code is alright. Go through the above three steps for better luck.
5. **Local developing tools such as ngrok usually produces large network latency for WeChat server to process. Try using a hosted machine for better network conditions.**

> You should understand WeChat's token authorization mechanism, which can be referred to as an OAuth implementation. Use `GET` method while authorizing, and `POST` for business logics (most of the time). As the WeChat server may request your server with `POST` method, it may cause CSRF problems for certain frameworks.

> If you have `Laravel-debugbar` enabled in your project, please disable it, as this component alters the content that replies to the WeChat server, making it impossible to resolve.

## Maximum function nesting level of '100' reached, aborting!

The Xdebug has a limitation of 100 nesting levels at most (by default).

To solve this, please find the `max_nesting_level` option in Xdebug's config, and change the value to a bigger number. 200 should be OK.

For example, you can use the config below. Do an Apache or PHP-FPM service restart to apply the change.
```
xdebug.max_nesting_level=200
```
