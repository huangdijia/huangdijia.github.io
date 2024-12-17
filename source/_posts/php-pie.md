---
title: PIE：新一代 PHP 扩展管理
date: 2024-12-17 09:45:31
tags: ["php", "pie"]
---

## 什么是 PIE

PIE（PHP Installer for Extensions） 是一个全新的工具，专为 `PHP` 开发者设计，目的是提供一个更轻量、更现代化的 `PHP` 扩展管理方式。它采用模块化设计，能够方便地添加、管理和部署 `PHP` 扩展。相比 `PECL`，`PIE` 的安装和使用过程更加流畅，具有更好的用户体验。

## 使用 PIE 的优势

与传统的 `PECL` 相比，`PIE` 更注重简洁和现代化的设计。其轻量级架构不仅减少了开发者的维护成本，还使得扩展的安装速度和稳定性得到了提升。

扩展开发者只需要在项目中增加 `composer.json`，声明一些安装选项等，并提交到 `Packagist` 即可。

<!--more-->

## 下载安装 PIE

需要 PHP 8.1 或更新版本才能运行 `PIE，但` `PIE` 可以为任何已安装的 `PHP` 版本安装扩展。

```shell
wget https://github.com/php/pie/releases/download/0.2.0/pie.phar

sudo chmod +x pie.phar
sudo mv pie.phar /usr/local/bin/pie
```

两种命令，二选一即可。

```shell
sudo curl -L --output /usr/local/bin/pie https://github.com/php/pie/releases/download/0.2.0/pie.phar && sudo chmod +x /usr/local/bin/pie
```

目前发布了 `0.2.0` 的预览版，所以本文使用 `0.2.0` 的下载地址，等正式版本发布后，可以使用 https://github.com/php/pie/releases/latest/download/pie.phar 此链接下载。

## 下载、构建或安装扩展

PIE 可以：

- 只下载一个扩展，使用 `pie download`
- 下载并构建扩展，使用 `pie build`
- 最常见的是：下载、构建和安装扩展，使用 `pie install`

使用 PIE 安装扩展时，必须使用其 Composer 软件包名称，可以在 https://packagist.org/extensions 上找到与 `PIE` 兼容的软件包列表。

知道扩展名称后，就可以使用下面的命令进行安装：

```shell
pie install <vendor>/<package>
```

举个例子

```shell
pie install apcu/apcu
```

在为不同的 PHP 版本安装扩展时，可以通过指定 `php-confi`g 来进行：

```shell
pie install --with-php-config=/usr/bin/php-config7.4 apcu/apcu
```

版本约束和 `composer` 是一样的，了解更多可以查看：`Composer` 进阶使用之版本约束表达式的使用

```shell
pie install <vendor>/<package>:<version-constraint>
```

编译扩展时，有些扩展需要向 `./configure` 命令传递额外的参数。

这些参数通常用于启用或禁用某些功能，或提供未自动检测到的库的路径。

为了确定扩展可用的配置选项，可以使用 `pie info <vendor>/<package>` 将返回列表，例如：

```shell
$ pie info apcu/apcu
You are running PHP 8.2.10
Target PHP installation: 8.2.10 nts, on Linux/OSX/etc x86_64 (from /usr/local/Cellar/php/8.2.10/bin/php)
Found package: apcu/apcu:v5.1.24 which provides ext-apcu
Extension name: apcu
Extension type: php-ext (PhpModule)
Composer package name: apcu/apcu
Version: v5.1.24
Download URL: https://api.github.com/repos/krakjoe/apcu/zipball/62e67989a35247263c370b5ecebb4e69b73b0709
Configure options:
    --disable-apcu-mmap  (Disable mmap, falls back on shm)
    --disable-apcu-rwlocks  (Disable rwlocks in APCu)
    --disable-valgrind-checks  (Disable Valgrind-based memory checks)
    --enable-apcu  (Enable APCu support)
    --enable-apcu-clear-signal  (Enable SIGUSR1 clearing handler)
    --enable-apcu-debug  (Enable APCu debugging)
    --enable-apcu-spinlocks  (Use spinlocks before flocks)
    --enable-coverage  (Include code coverage symbols (DEVELOPERS ONLY!!))
```

需要注意的是，目前，`PIE` 不会配置 `INI` 文件，但很快会进行改进，需要手动给对应的 `php.ini` 中添加 `extension=`

## 示例

- 安装 Swoole

```shell
pie install swoole/swoole:v6.0.0 \
--enable-sockets \
--enable-openssl \
--enable-mysqlnd  \
--enable-swoole-curl  \
--enable-iouring
```

- 安装 Redis

```shell
pie install phpredis/phpredis:v6.1.0 --enable-redis
```
