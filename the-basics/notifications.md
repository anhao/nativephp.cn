# 通知

### 原生通知

NativePHP 允许你使用优雅的 PHP API 发送系统通知。这些通知与 Laravel 内置的通知不同，它们是你的操作系统显示的实际 UI 通知。

适度使用时，通知是通知用户应用程序中发生的事件并将其注意力带回应用程序的好方法，尤其是在需要他们进一步输入的情况下。

#### 发送通知

你可以使用 `Notification` 门面发送通知。

```php
use Native\Laravel\Facades\Notification;

Notification::title('来自 NativePHP 的问候')
    ->message('这是来自你的 Laravel 应用的详细消息。')
    ->show();
```

这将向用户显示一个带有给定标题和消息的系统范围通知。

### 事件

#### 通知点击事件

你可以为 NativePHP 通知注册自定义事件。当用户点击通知时，将触发此事件，因此你可以在这种情况下在应用程序中添加一些自定义逻辑。

要向通知附加事件，你可以使用 `event` 方法。传递给此方法的参数是点击通知时应分派的事件的类名。

```php
use Native\Laravel\Facades\Notification;

Notification::title('来自 NativePHP 的问候')
    ->message('这是来自你的 Laravel 应用的详细消息。')
    ->event(\App\Events\MyNotificationEvent::class)
    ->show();
```
