# 应用程序生命周期

## NativePHP 应用生命周期

当您的 NativePHP 应用启动时 - 无论是在开发还是生产环境中 - 它会执行一系列步骤以使您的应用运行起来。

1. 原生 shell（Electron 或 Tauri）启动。
2. NativePHP 运行 `php artisan migrate` 以确保您的数据库是最新的。
3. NativePHP 运行 `php artisan serve` 以启动 PHP 开发服务器。
4. NativePHP 通过运行 `NativeAppServiceProvider` 上的 `boot()` 方法来启动您的应用。

除了 `boot()` 方法外，NativePHP 还会分派一个 `Native\Laravel\Events\App\ApplicationBooted` 事件。

### NativeAppServiceProvider

当运行 `php artisan native:install` 时，NativePHP 会发布一个 `NativeAppServiceProvider` 到 `app/Providers/NativeAppServiceProvider.php`。

您可以使用此服务提供者来引导您的应用。例如，您可能想要打开一个窗口、注册全局快捷方式或配置您的应用菜单。

默认的 `NativeAppServiceProvider` 看起来像这样：

```php
namespace App\Providers;

use Native\Laravel\Facades\Window;
use Native\Laravel\Contracts\ProvidesPhpIni;

class NativeAppServiceProvider implements ProvidesPhpIni
{
    /**
     * 一旦原生应用被启动，就会执行一次。
     * 使用此方法来打开窗口、注册全局快捷方式等。
     */
    public function boot(): void
    {
        Window::open();
    }

    /**
     * 返回要设置的 php.ini 指令数组。
     */
    public function phpIni(): array
    {
        return [
        ];
    }
}
```
