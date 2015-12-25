title: 贡献代码
---
## 开发

我们欢迎广大开发者贡献大家的智慧，让我们共同让它变得更完美.

### 开始之前

请严格遵循以下代码标准:

- [PSR-2](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md).
- 使用 4 个空格作为缩进。

### 流程

1. Fork [overtrue/wechat] 到本地.
2. 创建新的分支：

    {% code %}
    $ git checkout -b new_feature
    {% endcode %}

3. 编写代码。
4. Push 到你的分支:

    {% code %}
    $ git push origin new_feature
    {% endcode %}

5. 创建 Pull Request 并描述你完成的功能或者做出的修改。

### 注意

- 注释请使用英文

## 更新文档

我们的文档也是开源的，基于 [Hexo](http://hexo.io)，源代码在 [EasyWeChat/site](https://github.com/EasyWeChat/site)。

### 流程

1. Fork [EasyWeChat/site]
2. Clone 到你的电脑并且安装依赖包：

    {% code %}
    $ git clone https://github.com/<username>/site.git
    $ cd site
    $ npm install
    {% endcode %}

3. 编辑文档，同时你可以开启本地服务器实时预览：

    {% code %}
    $ hexo server
    {% endcode %}

4. Push 到你的分支。
5. 创建 Pull Request 并描述你完成的功能或者做出的修改。

### 多语言

1. 添加一个语言目录到 `source` 目录下. (小写目录名称)
2. 复制 `source` 下的所有文件到到你新建的语言目录下。
3. 编辑 `source/_data/language.yml`，添加你的语言。
4. 复制 `en.yml` 到 `themes/easywechat/languages` 并且改名 (全部小写).

## 报告 Bug

当你在使用过程中遇到问题，请查阅 [疑难解答](troubleshooting.html) 或者在这里提问 [GitHub](https://github.com/overtrue/wechat/issues). 如果还是不能解决你的问题，请到 GitHub 联系我们。

[overtrue/wechat]: https://github.com/overtrue/wechat