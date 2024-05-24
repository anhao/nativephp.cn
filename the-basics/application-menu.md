# 应用菜单

### 配置应用程序菜单

NativePHP 允许你配置应用程序的原生菜单，以及上下文菜单或 dock 菜单。你可以使用 `Menu` 门面，它为你提供了一个用于构建所有这些菜单的单一可重用抽象。

你的应用程序菜单配置应该在你的 `NativeAppServiceProvider` 的 `boot` 方法中进行。

#### 创建菜单

要创建新菜单，你可以使用 `Menu::new()` 方法。此方法返回菜单构建器，允许你添加其他项目或子菜单。配置菜单完成后，你可以调用 `register()` 方法将菜单注册到原生应用程序。

```php
namespace App\Providers;

use Native\Laravel\Facades\Window;
use Native\Laravel\Menu\Menu;

class NativeAppServiceProvider
{
    /**
     * 在原生应用程序启动后执行一次。
     * 使用此方法打开窗口、注册全局快捷键等。
     */
    public function boot(): void
    {
        Menu::new()
            ->appMenu()
            ->submenu('NativePHP', Menu::new()
                ->link('https://nativephp.com', '文档')
            )
            ->register();

        Window::open();
    }
}
```

### 预定义菜单

NativePHP 自带了一些开箱即用的预定义菜单。

#### 默认应用程序菜单

你可以使用 `appMenu()` 方法创建默认应用程序菜单。此菜单包含所有你期望在应用程序菜单中的默认项目（例如关于、服务、退出等）。

```php
Menu::new()
    ->appMenu()
    ->register();
```

`appMenu` 将使用你的应用程序名称作为其标题。

#### 默认编辑菜单

你可以使用 `editMenu()` 方法创建默认编辑菜单。此菜单包含所有你期望在编辑菜单中的默认项目及其功能（例如撤销、重做、剪切、复制、粘贴等）。

> `editMenu()` 启用许多常见的键盘快捷键（如剪切、复制、粘贴）。如果没有它，你需要自己定义这些快捷键。

```php
Menu::new()
    ->editMenu()
    ->register();
```

默认情况下，编辑菜单使用“编辑”作为其标题。你可以通过向 `editMenu()` 方法传递一个字符串来更改此标题。

```php
Menu::new()
    ->editMenu('我的编辑菜单')
    ->register();
```

#### 默认视图菜单

你可以使用 `viewMenu()` 方法创建默认视图菜单。此菜单包含所有你期望在视图菜单中的默认项目及其功能（例如切换全屏、切换开发者工具等）。

```php
Menu::new()
    ->viewMenu()
    ->register();
```

默认情况下，视图菜单使用“视图”作为其标题。你可以通过向 `viewMenu()` 方法传递一个字符串来更改此标题。

```php
Menu::new()
    ->viewMenu('我的视图菜单')
    ->register();
```

#### 默认窗口菜单

你可以使用 `windowMenu()` 方法创建默认窗口菜单。此菜单包含所有你期望在窗口菜单中的默认项目及其功能（例如最小化、缩放等）。

```php
Menu::new()
    ->windowMenu()
    ->register();
```

默认情况下，窗口菜单使用“窗口”作为其标题。你可以通过向 `windowMenu()` 方法传递一个字符串来更改此标题。

```php
Menu::new()
    ->windowMenu('我的窗口菜单')
    ->register();
```

#### 组合默认菜单

如果你想使用多个预定义菜单，只需将方法链接在一起。

```php
Menu::new()
    ->appMenu()
    ->editMenu()
    ->viewMenu()
    ->windowMenu()
    ->register();
```

### 自定义子菜单

你可以使用 `submenu()` 方法创建子菜单。此方法接受一个标题和一个菜单构建器作为其参数。

```php
Menu::new()
    ->appMenu()
    ->submenu('我的子菜单', Menu::new()
        ->link('https://nativephp.com', '文档')
    )
    ->register();
```

### 可用的子菜单项

#### 标签

NativePHP 允许你在菜单中添加标签。你可以使用 `label()` 方法向菜单添加标签。点击标签将触发 `Native\Laravel\Events\Menu\MenuItemClicked` 事件，该事件将被 广播。

```php
Menu::new()
    ->appMenu()
    ->submenu('我的子菜单', Menu::new()
        ->label('我的标签')
    )
    ->register();
```

#### 链接

你可以使用 `link()` 方法向菜单添加链接。此方法接受一个 URL 和一个标题作为其参数。当用户点击链接时，URL 将在默认浏览器中打开，并且将分派 `Native\Laravel\Events\Menu\MenuItemClicked` 事件。

事件的有效载荷将包含以下数据：

* `id`：菜单项的内部 ID。
* `label`：菜单项的标签。

```php
Menu::new()
    ->submenu('我的子菜单', Menu::new()
        ->link('https://nativephp.com', '了解更多')
    )
    ->register();
```

#### 分隔符

你可以使用 `separator()` 方法向菜单添加分隔符。分隔符是分隔菜单项的水平线。

```php
Menu::new()
    ->submenu('我的子菜单', Menu::new()
        ->link('https://nativephp.com', '了解更多')
        ->separator()
        ->link('https://nativephp.com', '文档')
    )
    ->register();
```

#### 基于事件的菜单项

事件菜单项在点击时自动触发指定事件。

```php
Menu::new()
    ->submenu('我的子菜单', Menu::new()
        ->event(App\Events\MyEvent::class, '触发我的事件')
    )
    ->register();
```

你可以在 `EventServiceProvider` 类中为你的自定义事件注册监听器。

```php
/**
 * 应用程序的事件监听器映射。
 *
 * @var array
 */
protected $listen = [
    App\Events\MyEvent::class => [
        'App\Listeners\MyMenuItemWasClicked',
    ],
    // ...
];
```

#### 复选框菜单项

你可以使用 `checkbox()` 方法向菜单添加复选框项。`checkbox()` 方法接受一个标签和一个布尔值作为复选框初始状态的参数。

当用户点击复选框项时，复选框将被切换，并且将分派 `Native\Laravel\Events\Menu\MenuItemClicked` 事件。

事件的有效载荷将包含以下数据：

* `id`：菜单项的内部 ID。
* `checked`：复选框是否被选中。
* `label`：菜单项的标签。

```php
Menu::new()
    ->submenu('我的子菜单', Menu::new()
        ->checkbox('我的复选框', true)
    )
    ->register();
```

#### 退出

你可以使用 `quit()` 方法向菜单添加退出项。当用户点击退出项时，应用程序将退出。

```php
Menu::new()
    ->submenu('我的子菜单', Menu::new()
        ->quit()
    )
    ->register();
```

### 热键

NativePHP 允许你为菜单项注册热键。`checkbox()`、`event()` 和 `link()` 方法接受一个热键作为其最后一个参数。

热键必须是一个包含修饰符和键的字符串，由 `+` 符号分隔。

例如，如果你想注册一个热键，当用户按下 `Cmd+Shift+D` 时触发 `MyEvent` 事件，你可以这样做：

```php
Menu::new()
    ->submenu('我的子菜单', Menu::new()
        ->event(App\Events\MyEvent::class, '触发我的事件', 'CmdOrCtrl+Shift+D')
    )
    ->register();
```

你可以在 全局热键文档部分 找到可用热键修饰符的列表。
