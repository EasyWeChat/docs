title: Contributing
---
## Development

We welcome you to join the development of EasyWeChat. This document will help you through the process.

### Before You Start

Please follow the coding style:

- Follow [PSR-2](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md).
- Use soft-tabs with a four space indent.

### Workflow

1. Fork [overtrue/wechat].
2. Create a feature branch.

    {% code %}
    $ git checkout -b new_feature
    {% endcode %}

3. Start hacking.
4. Push the branch:

    {% code %}
    $ git push origin new_feature
    {% endcode %}

5. Create a pull request and describe the change.

### Notice

TODO

## Updating Documentation

The EasyWeChat documentation is open source and you can find the source code on [EasyWeChat/site].

### Workflow

1. Fork [EasyWeChat/site]
2. Clone the repository to your computer and install dependencies.

    {% code %}
    $ git clone https://github.com/<username>/site.git
    $ cd site
    $ npm install
    {% endcode %}

3. Start editing the documentation. You can start the server for live previewing.

    {% code %}
    $ hexo server
    {% endcode %}

4. Push the branch.
5. Create a pull request and describe the change.

### Translating

1. Add a new language folder in `source` folder. (All lower case)
2. Copy Markdown and template files in `source` folder to the new language folder.
3. Add the new language to `source/_data/language.yml`.
4. Copy `en.yml` in `themes/easywechat/languages` and rename to the language name (all lower case).

## Reporting Issues

When you encounter some problems when using EasyWeChat, you can find the solutions in [Troubleshooting](troubleshooting.html) or ask me on [GitHub](https://github.com/overtrue/wechat/issues). If you can't find the answer, please report it on GitHub.

[overtrue/wechat]: https://github.com/overtrue/wechat
[EasyWeChat/site]: https://github.com/EasyWeChat/site