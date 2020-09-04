---
isChild: true
anchor:  mac_setup
title: Mac 安裝
---

## Mac 安装 {#mac_setup_title}

macOS 系统会预装 PHP， 只是一般情况下版本会比最新稳定版低一些。

以下介绍几种在 macOS 上安装最新版本 PHP 的方法。

### 通过 Homebrew 安装 PHP

[Homebrew] 是一个强大的 macOS 专用包管理器， 它可以帮助你轻松的安装 PHP 和各种扩展。
[Homebrew PHP] 是一个包含与 PHP 相关的 Formulae，能让你通过 homebrew 安装 PHP 的仓库。


也就是说, 你可以通过 `brew install` 命令安装 `php56`、`php70`、`php71`、`php72`、`php73` 或者 `php74`，使用以下命令安装最新版本：

    brew install php@7.4

您可以通过修改 `PATH` 变量在 Homebrew PHP 版本之间切换。或者你也可以使用 [brew-php-switcher][brew-php-switcher] 来自动切换。

### 通过 Macports 安装 PHP

[MacPorts] 是一个开源的，社区发起的项目，它的目的在于设计一个易于使用的系统，方便编译，安装以及升级 macOS 系统上的 command-line, X11 或者基于 Aqua 的开源软件。

MacPorts 支持预编译的二进制文件，因此你不必每次都重新从源码压缩包编译，如果你的系统没有安装这些包，它会节省你很多时间。


此时，你可以通过 `port install` 命名来安装 `php56`、`php70`、`php71`、`php72`、`php73` 或者 `php74`，比如：

    sudo port install php56
    sudo port install php74

你也可以执行 `select` 命令来切换当前的 php 版本：

    sudo port select --set php php74

### 通过 phpbrew 安装 PHP

[phpbrew] 是一个安装与管理多个 PHP 版本的工具。它在应用程序或者项目需要不同版本的 PHP 时非常有用，让你不再需要使用虚拟机来处理这些情况。

### 通过 Liip's binary installer 安装 PHP
[php-osx.liip.ch] 是另一种流行的选择，它提供了从 5.3 到 7.4 版本的单行安装功能。
它并不会覆盖Apple集成的PHP文件，而是将其安装在了一个独立的目录中(/usr/local/php5)。

### 源码编译

另一个让你控制安装 PHP 版本的选择就是 [自行编译][mac-compile]。
如果使用这种方法， 你必须先确认是否已经通过 Apple 的 Mac 开发人员中心下载、安装 [Xcode][xcode-gcc-substitution] 或者 ["XCode命令行工具(Command Line Tools for XCode)"][Command Line Tools for XCode]。

### 集成环境 (All-in-One Installers)

上面列出的解决方案主要是针对 PHP 本身， 并不包含：比如 [Apache]，[Nginx] 或者 SQL Server 之类的东西。。
集成环境比如 [MAMP][mamp-downloads] 和 [XAMPP][xampp] 会安装这些软件并且将他们绑在一起，不过易于安装的背后也牺牲了一定的弹性。


[Homebrew]: http://brew.sh/
[Homebrew PHP]: https://github.com/Homebrew/homebrew-php#installation
[brew-php-switcher]: https://github.com/philcook/brew-php-switcher
[MacPorts]: https://www.macports.org/install.php
[phpbrew]: https://github.com/phpbrew/phpbrew
[php-osx.liip.ch]: https://php-osx.liip.ch/
[mac-compile]: https://secure.php.net/install.macosx.compile
[xcode-gcc-substitution]: https://github.com/kennethreitz/osx-gcc-installer
[Command Line Tools for XCode]: https://developer.apple.com/downloads
[Apache]: https://httpd.apache.org/
[Nginx]: https://www.nginx.com/
[mamp-downloads]: http://www.mamp.info/en/downloads/
[xampp]: http://www.apachefriends.org/en/xampp.html
