# 窗口

### 使用窗口

NativePHP 允许你打开原生应用程序窗口。通常这会在你的 `NativeAppServiceProvider` 中发生，但你也可以在应用程序的任何地方打开窗口。

#### 打开窗口

要打开一个窗口，你可以使用 `Window` 门面。

```php
namespace App\Providers;

use Native\Laravel\Facades\Window;

class NativeAppServiceProvider
{
    public function boot(): void
    {
        Window::open()
            ->width(800)
            ->height(800);
    }
}
```

当打开窗口时，NativePHP 会自动打开你的应用程序的根 URL。你可以向 `open()` 方法传递一个唯一标识符来区分多个窗口。

如果没有指定 ID，默认 ID 是 `main`。

你可以使用这个 ID 在其他方法中引用窗口，例如 `Window::close()` 或 `Window::resize()`。

#### 关闭窗口

要关闭一个窗口，你可以使用 `Window::close()` 方法。你可以向 `close()` 方法传递一个唯一标识符来指定要关闭的窗口。

如果你没有指定窗口 ID，NativePHP 会尝试根据当前路由自动检测窗口 ID。

```php
Window::close();
Window::close('settings');
```

#### 调整窗口大小

你可以使用 `Window::resize()` 方法来调整窗口大小。这个方法接受宽度和高度作为它的前两个参数。

你可以向 `resize()` 方法传递一个唯一标识符来指定要调整大小的窗口。

如果你没有指定窗口 ID，NativePHP 会尝试根据当前路由自动检测窗口 ID。

```php
Window::resize(400, 300);

Window::resize(400, 300, 'settings');
```

#### 获取当前窗口

你可以使用 `Window::current()` 方法来获取当前聚焦的窗口。这个方法返回一个具有以下属性的对象：

* `id`：窗口的 ID。
* `title`：窗口的标题。
* `width`：窗口的宽度。
* `height`：窗口的高度。
* `x`：窗口的 x 位置。
* `y`：窗口的 y 位置。
* `alwaysOnTop`：窗口是否始终在最上面。

```php
$currentWindow = Window::current();
```

### 管理多个窗口

如果你想打开多个窗口，你可以多次使用 `Window::open()` 方法。为了区分各个窗口，你可以向 `open()` 方法传递一个唯一标识符。

如果你没有指定 ID，NativePHP 会自动使用 `main` 作为 ID。

这个 ID 可以用于在其他方法中引用窗口，例如 `Window::close()` 或 `Window::resize()`。

```php
Window::open('home')
    ->width(800)
    ->height(800);

Window::open('settings')
    ->route('settings')
    ->width(800)
    ->height(800);
```

### 配置窗口

#### 窗口 URL

默认情况下，所有对 `Window::open()` 的调用都会打开你的应用程序的根 URL。如果你想打开不同的 URL，你可以使用 `route()` 方法来指定要打开的路由名称。

```php
Window::open()
    ->route('home');
```

你也可以向 `url()` 方法传递一个绝对 URL：

```php
Window::open()
    ->url('https://google.com');
```

#### 窗口标题

默认情况下，所有对 `Window::open()` 的调用都会使用应用程序名称作为窗口标题。如果你想使用不同的标题，你可以使用 `title()` 方法来指定要使用的窗口标题。

```php
Window::open()
    ->title('我的窗口');
```

#### 窗口大小

你可以使用 `width()` 和 `height()` 方法来指定窗口的大小。

```php
Window::open()
    ->width(800)
    ->height(800);
```

如果你想将窗口限制在特定大小，你可以使用 `minWidth()`, `minHeight()`, `maxWidth()`, 和 `maxHeight()` 方法。

```php
Window::open()
    ->minWidth(400)
    ->minHeight(400)
    ->maxWidth(800)
    ->maxHeight(800);
```

#### 窗口位置

要指定窗口的位置，你可以使用 `position($x, $y)` 方法。

```php
Window::open()
    ->position(100, 100);
```

#### 记住窗口状态

你的应用程序的用户可能会调整窗口大小或移动窗口，并期望下次打开时它处于相同的位置和大小。NativePHP 提供了一种简单的方法来管理窗口的状态。你可以使用 `rememberState()` 方法来指示 NativePHP 记住窗口的状态。

```php
Window::open()
    ->rememberState();
```

请注意，NativePHP 只允许你一次记住一个窗口的状态。

#### 可调整大小的窗口

默认情况下，所有使用 `Window` 门面创建的窗口都是可调整大小的。如果你想禁用调整大小，你可以使用 `resizable()` 方法并传递 `false` 作为第一个参数。

```php
Window::open()
    ->resizable(false);
```

#### 可聚焦的窗口

默认情况下，所有使用 `Window` 门面创建的窗口都可以通过单击它们来聚焦。你可以使用 `focusable()` 方法来禁用聚焦。

```php
Window::open()
    ->focusable(false);
```

#### 可移动的窗口

默认情况下，所有使用 `Window` 门面创建的窗口都是可移动的。你可以使用 `movable()` 方法来禁用移动。

```php
Window::open()
    ->movable(false);
```

#### 可最小化、最大化、关闭的窗口

默认情况下，所有使用 `Window` 门面创建的窗口都是可最小化、可最大化和可关闭的。

你可以使用 `minimizable()`, `maximizable()`, 和 `closable()` 方法来禁用这些功能。

```php
Window::open()
    ->minimizable(false)
    ->maximizable(false)
    ->closable(false);
```



### 事件

NativePHP 提供了一种简单的方法来监听原生窗口事件。所有事件都作为常规 Laravel 事件分派，因此你可以使用你的 `EventServiceProvider` 来注册监听器。

有时你可能想要实时监听并响应窗口事件，这就是为什么 NativePHP 还将所有窗口事件广播到 `nativephp` 广播频道。

要了解更多关于 NativePHP 的广播功能，请参考 广播 部分。

#### 窗口显示事件

当窗口显示给用户时，将分派 `Native\Laravel\Events\Windows\WindowShown` 事件。此事件的有效载荷包含窗口 ID。

#### 窗口关闭事件

当窗口关闭时，将分派 `Native\Laravel\Events\Windows\WindowClosed` 事件。此事件的有效载荷包含窗口 ID。

#### 窗口聚焦事件

当窗口获得焦点时，将分派 `Native\Laravel\Events\Windows\WindowFocused` 事件。此事件的有效载荷包含窗口 ID。

#### 窗口失去焦点事件

当窗口失去焦点时，将分派 `Native\Laravel\Events\Windows\WindowBlurred` 事件。此事件的有效载荷包含窗口 ID。

#### 窗口最小化事件

当窗口最小化时，将分派 `Native\Laravel\Events\Windows\WindowMinimized` 事件。此事件的有效载荷包含窗口 ID。

#### 窗口最大化事件

当窗口最大化时，将分派 `Native\Laravel\Events\Windows\WindowMaximized` 事件。此事件的有效载荷包含窗口 ID。

#### 窗口调整大小事件

在窗口调整大小后，将分派 `Native\Laravel\Events\Windows\WindowResized` 事件。此事件的有效载荷包含窗口 ID 以及新的窗口宽度和高度。

你可以在你的 `EventServiceProvider` 中为这些事件注册监听器：

```php
/**
 * 应用程序的事件监听器映射。
 *
 * @var array
 */
protected $listen = [
    'Native\Laravel\Events\Windows\WindowShown' => [
        'App\Listeners\WindowWasShownListener',
    ],
    // ...
];
```
