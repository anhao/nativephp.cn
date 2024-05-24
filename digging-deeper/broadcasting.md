# 广播

## 广播

NativePHP 在其操作过程中触发各种事件。你可以像往常一样使用应用程序中的任何事件监听器来监听这些事件。

### 实时响应 NativePHP 事件

为了实时响应 NativePHP 事件，例如聚焦窗口，NativePHP 以两种不同的方式广播这些事件：

#### WebSockets

NativePHP 通过 `nativephp` 广播频道上的 websocket 连接广播事件。这允许你使用 Laravel Echo 实时监听这些事件，并在你的应用程序前端或通过 Laravel Livewire 响应这些事件。

为了在你的 NativePHP 应用程序中使用 WebSockets，你可以在你的 Laravel 应用程序中安装 [Laravel WebSockets](https://beyondco.de/docs/laravel-websockets) 包。NativePHP 会在你的应用程序运行时自动运行 `php artisan websockets:serve` 来启动你的 WebSocket 服务器，允许你使用 Laravel Echo 连接到它并实时监听事件。

#### 浏览器事件

除了使用 Laravel Echo 连接到本地 WebSocket 服务器之外，你还可以使用 Electron 的 [Inter-Process-Communication System](https://electronjs.org/docs/latest/api/ipc-renderer) 通过 JavaScript 实时监听所有内部 PHP 事件。这特别有用，因为它减少了通过 WebSocket 连接分派和监听 Laravel 事件的开销和延迟。

#### JavaScript

你可以使用 Electron 的 `ipcRenderer` API 实时监听这些事件。此 API 分派一个 `native-event` 事件，该事件包含以下有效负载：

* `event` - 一个字符串，包含已分派的 NativePHP 事件类的完全限定类名
* `payload` - 一个包含事件有效负载的对象

```js
import * as remote from '@electron/remote'

ipcRenderer.on('native-event', (_, data) => {
    const eventClassName = data.event;
    const eventPayload = data.payload;
});
```

#### Livewire

当使用 [Livewire](https://laravel-livewire.com) 时，为了使这个过程更容易，你可以在监听事件时使用 `native:` 前缀。这与 [使用 Livewire 监听 Laravel Echo 事件](https://laravel-livewire.com/docs/2.x/laravel-echo) 类似。

```php
class AppSettings extends Component
{
    public $windowFocused = true;
    
    // 特殊语法：['native:{event}' => '{method}']
    protected $listeners = [
        'native:'.\Native\Laravel\Events\Windows\WindowFocused::class => 'windowFocused',
        'native:'.\Native\Laravel\Events\Windows\WindowBlurred::class => 'windowBlurred',
    ];
    
    public function windowFocused()
    {
        $this->windowFocused = true;
    }
    
    public function windowBlurred()
    {
        $this->windowFocused = false;
    }
}
```

### 可用事件

所有由 NativePHP 触发和广播的事件的完整列表可以在 [src/Events/](https://github.com/nativephp/laravel/tree/main/src/Events) 文件夹中找到。

### 广播自定义事件

你也可以广播你自己的自定义事件。只需让你的事件实现 `ShouldBroadcastNow` 契约，并在你的事件中定义 `broadcastOn` 方法，返回 `nativephp` 作为它广播到的频道之一：

```php
use Illuminate\Broadcasting\Channel;
use Illuminate\Contracts\Broadcasting\ShouldBroadcastNow;

class JobFinished implements ShouldBroadcastNow
{
    public function broadcastOn(): array
    {
        return [
            new Channel('nativephp'),
        ];
    }
}
```

当你想要将一个密集的任务卸载到后台队列并等待其完成，而不需要不断轮询你的应用程序以获取其状态时，这非常有用。

你触发的事件将被广播，你的应用程序可以像往常一样监听它。
