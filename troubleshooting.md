title: Troubleshooting
---

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


## 支付失败！当前页面的 URL 未注册 (Payment failed! Current URL is not registered)

This is a common mistake on the developer side. You need to configure the authorized directory in WeChat Plubic Platform. The options are located in **【微信支付】->【开发设置】(WeChat Payment -> Development Config)**.

Note: 

1. WeChat supports 3 authorized directories at most, which may be helpful for your multiple apps based on a single official account.
2. The correct format for **【支付授权目录】(authorized directory)** must start with `http://` or `https://`, and end with a slash `/`. Plus, the domain name must have an ICP license number.
3. The authorized directory should contain a **two-level or three-level directory** if needed.
4. All pages that actually send payment requests **must be under the path of your authorized directories**.
5. Additionally, if you need to test your payment functionality, you need to add your test WeChat account into the whitelist. Otherwise you may encounter some errors.

> You may want to learn some basic terms about web app development beforehand, e.g. **page**, **directory**, **URL** and **domain names** etc. Skip this info if you're experienced. :smile:

## redirect_url 参数错误 (redirect_url params error)

This is due to your app used **web authorization** without properly configuring the **【网页授权域名】(web authorization domain name)**. Please login to [WeChat Public Platform](https://mp.weixin.qq.com/), and find the **网页授权获取用户基本信息 (Web Authorization to obtain basic user information)** options under **【开发】->【接口权限】(Development -> API Permissions)**.

1. The domain name must have an ICP license number. Or it could not be saved.
2. The **web authorization domain name** means the url to redirect to after you got the authorization **code**. In most occasions, it is your business domain name.
3. It will take effect immediately after you have saved the domain name.
4. WeChat supports only 1 web authorization domain name. So you may need some extra work planning your web app.

## [JSAPI] config: invalid url domain

在使用 JS-SDK 进行开发时，每个页面都需要调用 wx.config() 方法配置 JSPAI 参数。如果没有正确配置 **JSAPI 安全域名**并且开启了调试模式，此时就报此错误。遇到这个问题时，开发者需要登录微信公众平台，进入【公众号设置】->【功能设置】页面，将项目所使用的域名添加至 **【JSAPI 安全域名】**列表中。

1. 一个公众号同时最多可绑定**三个**安全域名，并且这些域名必须为通过 **ICP 备案**的**一级或一级以上**的有效域名。

2. JSAPI 安全域名每个月**限修改三次**，修改任何一个都算，所以，请谨慎操作。

3. 如果需要使用 JSAPI 调起支付功能，则支付目录必须也在所配置的**安全域名之下**，并且需要将支付目录添加至**支付授权目录**。

## token验证失败、向公众号发送消息无任何反应

相信对接公众号一般是微信开发者进行开发过程中最先进行的工作，而在这看似简单的配置操作中，也可能会掉坑里。
最常见的两种情况就如下：

1. 确认你 “**启用**” 了开发模式， token 验证通过不代表启用，保存后也不代表启用。看到红色 “**停用**” 才真正的是启用了。

2. 配置好URL(服务器地址)以及Token(令牌)后，点击保存时提示**token验证失败**，出现这种情况的原因有多种，其中之一便是网络不稳定，所以**可尝试多次保存**，若始终无法通过再排查其它可能因素。

3. 配置保存成功之后，向公众号发送消息无任何反应，自己的消息处理程序也没有被调用的记录（无对应日志）。这种情况下如果你尝试**反复停用和启用服务器配置**，可能突然间惊奇地了现，问题莫名其妙的解决了。

4. 使用在线调试工具的消息接口，http://mp.weixin.qq.com/debug/， 只要返回绿色的“**请求成功**”，就代表你的代码没有问题，请**重复上面第4项**再测试。

5. **如果你在用什么本地开发工具，或者什么 ngrok 代理到本机这样的开发方式，那么失败就很正常了，微信服务器到你机器的网络延迟太大（还是用服务器开发吧）。**

> 请开发者理解服务器 TOKEN 验证原理（官方文档有说明）并谨记服务器验证时使用 GET 方式访问，而公众平台向你的服务器发送消息/数据则使用 POST 方式，所以服务器验证成功之后，在某些启用了 CSRF 验证的框架里，接收消息时可能还会遇到 CSRF 相关的问题，请根据自己项目实际情况进行排查。
> 另外有的朋友的 Laravel 里使用了 laravel-debugbar，这个组件的原理是在页面输出时在后面添加 HTML 来实现的，所以它会改变我们返回给微信的内容，此时要么卸载，要么禁用掉它。


## Maximum function nesting level of '100' reached, aborting!

在使用了 Xdebug 的环境下可能出现这个问题。这是由于 Xdebug 限制函数嵌套的最大层级数（默认为100），当嵌套次数达到该值便会触发 Xdebug 跳出嵌套并报此错误。

为避免这个问题，**可以将 Xdebug 的 max_nesting_level 参数适当设置大一些**，通常设置为200就可以了（当然可根据自己实际情况设置为更大的值）。

如下，修改 php.ini 配置文件后，重启 Apache 或 php-fpm 服务即可。

```
xdebug.max_nesting_level=200
```
