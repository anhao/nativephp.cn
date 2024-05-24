# 剪切板

### 剪贴板操作

NativePHP 允许你使用 `Clipboard` 门面轻松地从系统剪贴板读取和写入数据，只需使用 PHP。

#### 从剪贴板读取数据

你可以使用适当的方法从剪贴板读取 `text`、`html` 或 `image` 数据：

```php
use Native\Laravel\Facades\Clipboard;

Clipboard::text();
Clipboard::html();
Clipboard::image();
```

#### 向剪贴板写入数据

你可以使用适当的方法向剪贴板写入 `text`、`html` 或 `image` 数据：

```php
use Native\Laravel\Facades\Clipboard;

Clipboard::text('复制的一些文本');
Clipboard::html('<div>复制的一些 HTML</div>');
Clipboard::image('path/to/image.png');
```

注意，`image()` 方法期望一个图像路径，而不是图像数据本身。NativePHP 将为你处理图像数据的序列化。

#### 清除剪贴板

你还可以使用 `clear()` 方法以编程方式清除剪贴板。

```php
use Native\Laravel\Facades\Clipboard;

Clipboard::clear();
```

如果你需要剪贴板内容在一定时间后过期，这非常有用。
