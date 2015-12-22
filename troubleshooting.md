title: 疑难解答
---

在微信公众平台开发的道路上，遍布着各种大大小小的坑，有的人掉坑里，几经折腾又爬出来了，然后拍拍屁股走人。然而坑还在那里，还会继续有后来人掉进去……  

这，是我们不愿看到的。  

所以在这里，我们将陆续将微信开发中可能遇到的各种疑难问题进行汇总，并给出对应的解决办法。一般情况下，这些问题都可以对号入座，轻松地解决。但也不排除特殊情况，这时候你遇到的问题与文中某一个症状一致，但文中所给的解决方案并不凑效，这种情况下就需要发挥你自己的智慧，去……折腾了……  

我们期待这一版块为各位的开发带来便利，同时也希望各位本着开源、分享的精神对其进行补充和完善，将各种坑一一填小、填平，让微信开发变得不那么痛苦，甚至，变成一件快乐的事……


### curl: (60) SSL certificate problem: unable to get local issuer certificate

这是 SSL 证书问题所致，在使用SDK调用微信支付等相关的操作时可能会遇到报 “SSL certificate problem: unable to get local issuer certificate” 的错误。  

微信公众平台提供的文档中建议对部分较敏感的操作接口使用 https 协议进行访问，例如微信支付和红包等接口中涉及到操作商户资金的一些操作。  
wecaht SDK 遵循了官方建议，所以在调用这些接口时，除了按照官方文档设置操作证书文件外，还需要保证服务器正确安装了 CA 证书。  

下载 CA 证书：http://curl.haxx.se/ca/cacert.pem 或者 使用[微信官方提供的证书](https://pay.weixin.qq.com/wiki/doc/api/app.php?chapter=4_3)中的 CA 证书 `rootca.pem` 也是同样的效果。

配置CA证书十分简单。只需要将上面下载好的CA证书（如果从商户平台下载的证书文件，里面有一个rootca.pem证书可直接使用），然后修改 `php.ini` 的 `curl.cainfo` ，然后重启 `php-fpm` 服务即可。注意证书文件路径以自己实际情况为准。

```
curl.cainfo = /path/to/downloaded/cacert.pem 
```

其它修改 HTTP 类源文件的方式是不允许的。
