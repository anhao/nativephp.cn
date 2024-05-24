# 安装

## 要求

1. PHP 8.1+
2. Laravel 10 或更高版本
3. Node 20+
4. Windows 10+ / macOS 12+ / Linux

### PHP & Node

在开发机器上直接运行 PHP 和 Node 是 NativePHP 最好的开发体验。

如果你使用的是 Mac 或 Windows，最简便的方式是使用 [Laravel Herd](https://herd.laravel.com) 来安装 PHP 和 Node。它快速而且免费！

请注意，虽然你可以从虚拟化环境或容器中开发和运行应用，但可能会遇到更多意外问题，并且需要更多手动步骤来创建工作构建。

### Laravel

NativePHP 最适用于与 Laravel 配合使用。你可以将其安装到现有的 Laravel 应用程序中，或者[开始一个新的应用](https://laravel.com/docs/10.x/installation)。

### 安装 NativePHP 运行时

```bash
composer require nativephp/electron
```

Tauri 运行时即将推出。

### 运行 NativePHP 安装程序

```bash
php artisan native:install
```

NativePHP 安装程序会发布 NativePHP 服务提供者，用于启动应用程序与所使用的运行时（Electron 或 Tauri）所需的依赖项。它还会发布 NativePHP 配置文件到 `config/nativephp.php`。

最后，它会安装所使用运行时需要的其他依赖项，比如对于 Electron，它会安装 NPM 依赖项。

**每当在新机器或 CI 中设置 NativePHP 时，你应该运行安装程序，以确保所有构建应用程序所需的依赖项都得到满足。**

### 启动开发服务器

```bash
php artisan native:serve
```

这将启动本地测试的应用程序开发构建。

到此为止！现在你应该能够在本地窗口中看到你的 Laravel 应用程序在原生桌面上运行。
