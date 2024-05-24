# 菜单栏

### 使用菜单栏

NativePHP 允许你为你的应用程序创建一个原生应用程序菜单栏。这可以作为你现有应用程序的补充，该应用程序已经使用窗口，或者作为一个独立的（仅菜单栏）应用程序。

当用户点击菜单栏图标时，菜单栏窗口将打开并显示给定的 URL 或路由。

你的 MenuBar 配置应该在你的 `NativeAppServiceProvider` 的 `boot` 方法中进行。

#### 创建独立的菜单栏应用程序

要为你的应用程序创建菜单栏，你可以使用 `MenuBar` 门面。创建菜单栏时，NativePHP 会自动打开你的应用程序的根 URL。默认情况下，添加菜单栏会自动隐藏你的应用程序的 dock 图标。

```php
namespace App\Providers;

use Native\Laravel\Facades\MenuBar;

class NativeAppServiceProvider
{
    public function boot(): void
    {
        MenuBar::create();
    }
}
```

#### 为已经使用窗口的应用程序创建菜单栏

你也可以为已经使用窗口的应用程序创建菜单栏。通常在这种情况下，你会希望显示你的应用程序的 dock 图标。为此，你可以使用 `MenuBar::create()` 方法，但这次调用 `showDockIcon()` 方法。

```php
namespace App\Providers;

use Native\Laravel\Facades\MenuBar;

class NativeAppServiceProvider
{
    public function boot(): void
    {
        MenuBar::create()
            ->showDockIcon();
    }
}
```

#### 打开菜单栏

你可以使用 `MenuBar::show()` 方法手动打开菜单栏窗口。

```php
MenuBar::show();
```

#### 隐藏菜单栏

你可以使用 `MenuBar::hide()` 方法手动关闭菜单栏窗口。

```php
MenuBar::hide();
```

#### 菜单栏标签

默认情况下，菜单栏将仅显示配置的 菜单栏图标。此外，你可以向菜单栏添加一个标签，该标签将显示在图标旁边。

你可以随时使用 `label()` 方法更改此标签。

```php
MenuBar::label('状态：在线');
```

你也可以在创建菜单栏时使用 `label()` 方法设置初始标签。

```php
MenuBar::create()
    ->label('状态：在线');
```

要删除标签，你可以向 `label()` 方法传递一个空字符串。

```php
MenuBar::label('');
```

### 配置菜单栏

#### 菜单栏 URL

默认情况下，`MenuBar::create()` 方法将配置你的菜单栏，当点击时显示你的应用程序的根 URL。如果你想打开不同的 URL，你可以使用 `route()` 方法来指定要打开的路由名称。

```php
MenuBar::create()
    ->route('home');
```

你也可以向 `url()` 方法传递一个绝对 URL：

```php
MenuBar::create()
    ->url('https://google.com');
```

#### 菜单栏图标

默认的菜单栏图标是 NativePHP 徽标。你可以使用 `icon()` 方法更改此图标。此方法接受一个指向图像文件的绝对路径。

在提供图标时，你应该确保图像是带有透明背景的 PNG 文件。图标的推荐大小是 **22x22 像素**，以及 **44x44 像素** 用于 retina 显示屏。

retina 显示屏图标的文件名应与常规图标相同，但文件名后附加 `@2x`。

示例：

```
menuBarIcon.png
menuBarIcon@2x.png
```

在 macOS 上，建议使用所谓的“模板图像”。这是一种渲染为带有透明背景的白色或黑色图像。

NativePHP 可以自动将你的图像转换为模板图像。为此，你可以将 `Template` 附加到图像文件名。

示例：

```
menuBarIconTemplate.png
menuBarIconTemplate@2x.png
```

你不需要手动将 `@2x` 附加到文件名，因为 NativePHP 会自动检测 retina 显示屏图标并在可用时使用它。

```php
MenuBar::create()
    ->icon(storage_path('app/menuBarIconTemplate.png'));
```

#### 菜单栏窗口大小

默认的菜单栏窗口大小是 **400x400 像素**。你可以使用 `width()` 和 `height()` 方法来指定用户点击菜单栏图标时将打开的窗口大小。

```php
MenuBar::create()
    ->width(800)
    ->height(600);
```

#### 菜单栏始终在最上面

在开发菜单栏应用程序时，你可能希望确保菜单栏窗口始终打开并位于所有其他窗口之上。这使得开发你的应用程序更容易，因为你不必每次想查看窗口时都点击菜单栏图标。

为此，你可以在 `MenuBar` 上使用 `alwaysOnTop()` 方法。

```php
MenuBar::create()
    ->alwaysOnTop();
```

### 菜单栏上下文菜单

你可以向菜单栏图标添加上下文菜单。当用户右键点击菜单栏图标时，将显示此上下文菜单。

#### 添加上下文菜单

要添加上下文菜单，你可以在 `MenuBar` 上使用 `contextMenu()` 方法。此方法接受一个 `Native\Laravel\Menu\Menu` 实例。

要了解更多关于菜单构建器的信息，请参考 菜单构建器 文档。

```php
MenuBar::create()
    ->withContextMenu(
        Menu::new()
            ->label('我的应用程序')
            ->separator()
            ->link('https://nativephp.com', '了解更多…')
            ->separator()
            ->quit()
    );
```

### 菜单栏事件

NativePHP 提供了一种简单的方法来监听菜单栏事件。所有事件都作为常规 Laravel 事件分派，因此你可以使用你的 `EventServiceProvider` 来注册监听器。

有时你可能想要实时监听并响应窗口事件，这就是为什么 NativePHP 还将所有窗口事件广播到 `nativephp` 广播频道。

要了解更多关于 NativePHP 的广播功能，请参考 广播 部分。

#### 菜单栏打开

当用户点击菜单栏图标并且菜单栏窗口打开时，或者当使用 `MenuBar::show()` 方法显示菜单栏时，将分派 `Native\Laravel\Events\MenuBar\MenuBarShown` 事件。

#### 菜单栏关闭

当用户点击菜单栏窗口之外并且菜单栏窗口关闭时，或者当使用 `MenuBar::hide()` 方法隐藏菜单栏时，将分派 `Native\Laravel\Events\MenuBar\MenuBarHidden` 事件。

#### 菜单栏上下文菜单打开

当用户右键点击菜单栏图标并且上下文菜单打开时，将分派 `Native\Laravel\Events\MenuBar\MenuBarContextMenuOpened` 事件。
