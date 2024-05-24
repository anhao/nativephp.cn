# 介绍

## 环境要求

1. PHP 8.1+
2. Laravel 10 or higher
3. Node 20+
4. Windows 10+ / macOS 12+ / Linux

### PHP＆Node

对于 NativePHP 来说，最佳的开发体验就是直接在开发机上运行 PHP 和 Node。

如果你使用的是 Mac 或 Windows，获得 PHP 和 Node 运行在系统上最简便的方法是使用 [Laravel Herd](https://herd.laravel.com)。它又快又免费！

请注意，尽管可以从虚拟化环境或容器中开发和运行应用程序，但可能会遇到更多意外问题，并需要更多手动步骤以创建可用构建。

### Laravel

NativePHP 是专为与 Laravel 最佳配合而构建的。您可以将其安装到现有的 Laravel 应用程序中，或者[启动一个新的应用程序](https://laravel.com/docs/10.x/installation)。

### 安装 NativePHP 运行时

```bash
composer require nativephp/electron
```

Tauri 运行时即将推出。

### 运行 NativePHP 安装程序

```bash
php artisan native:install
```

NativePHP 安装程序会负责发布 NativePHP 服务提供程序，该提供程序为您的应用程序与正在使用的运行时（Electron 或 Tauri）打造所需的依赖关系。它还会发布 NativePHP 配置文件至`config/nativephp.php`。

最后，它会安装您正在使用的特定运行时所需的其他任何依赖项，例如，对于Electron，它会安装 NPM 依赖项。

**每当您在新机器上或在 CI 中设置 NativePHP 时，应运行安装程序，确保所有必要的依赖项都已准备就绪以构建您的应用程序。**

### 启动开发服务器

```bash
php artisan native:serve
```

这会为本地测试启动您的应用程序的开发版本。

就是这样！您现在应该在本机窗口中看到您的 Laravel 应用程序正在运行。
