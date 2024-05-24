# 全局热键

### 全局热键

在您的 NativePHP 应用程序中，您可以定义多个全局热键。与您可能通过 JavaScript 在应用程序中定义的热键不同，这些热键是全局注册的。这意味着即使您的应用程序在后台运行且未聚焦，它也会收到这些热键。

由于这些全局热键通常用于整个应用程序，因此注册它们的一种常见方法是在 `NativeAppServiceProvider` 类中。

```php
namespace App\Providers;

use Native\Laravel\Facades\GlobalShortcut;

class NativeAppServiceProvider
{
    public function boot(): void
    {
        GlobalShortcut::key('CmdOrCtrl+Shift+A')
            ->event(\App\Events\MyShortcutEvent::class)
            ->register();
            
        // 其他代码，例如注册菜单、打开窗口等。
    }
}
```

### 注册热键

您可以使用 `GlobalShortcut` 门面注册全局快捷键。使用 `key` 方法，您可以指定要监听的热键。热键必须是一个包含修饰符和键的字符串，由 `+` 符号分隔。

例如，如果您想注册一个热键，当用户按下 `Cmd+Shift+D` 时触发 `MyEvent` 事件，您可以这样做：

```php
GlobalShortcut::key('Cmd+Shift+D')
    ->event(\App\Events\MyEvent::class)
    ->register();
```

您可以在这里找到所有可用修饰符的列表。

### 移除已注册的热键

有时您可能想要移除已注册的全局热键。为此，指定您用于注册的热键，并在 `GlobalShortcut` 门面上调用 `unregister` 方法。在这种情况下，您不需要提供事件类，因为每个热键只能注册一次。

例如，为了移除 `Cmd+Shift+D` 全局热键，您可以这样做：

```php
GlobalShortcut::key('Cmd+Shift+D')
    ->unregister();
```

#### 可用修饰符

* `Command` 或 `Cmd`
* `Control` 或 `Ctrl`
* `CommandOrControl` 或 `CmdOrCtrl`
* `Alt`
* `Option`
* `AltGr`
* `Shift`
* `Super`
* `Meta`

#### 可用键码

* `0` 到 `9`
* `A` 到 `Z`
* `F1` 到 `F24`
* `Backspace`
* `Delete`
* `Insert`
* `Return` 或 `Enter`
* `Up`, `Down`, `Left` 和 `Right`
* `Home` 和 `End`
* `PageUp` 和 `PageDown`
* `Escape` 或 `Esc`
* `VolumeUp`, `VolumeDown` 和 `VolumeMute`
* `MediaNextTrack`, `MediaPreviousTrack`, `MediaStop` 和 `MediaPlayPause`
* `PrintScreen`
* `Numlock`
* `Scrolllock`
* `Space`
* `Plus`
