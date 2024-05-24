# 对话框

### 原生对话框

NativePHP 允许你打开原生的文件对话框。它们可以用来让用户选择文件或文件夹，或者保存文件。

#### 打开文件对话框

要打开文件对话框，你可以使用 `Dialog` 类及其 `open()` 方法。

`open()` 方法的返回值是用户选择的文件或文件夹的路径。这可能是 null、文件路径（字符串）或文件路径数组，具体取决于你打开的对话框类型。

```php
use Native\Laravel\Dialog;

Dialog::new()
    ->title('选择文件')
    ->open();
```

#### 打开保存对话框

`open()` 对话框允许用户选择现有的文件或文件夹，但不允许创建新的文件或文件夹。为此，你可以使用 `save()` 方法。此方法将返回用户想要保存的文件的路径。

请注意，`save()` 方法实际上不会为你保存文件，它只会返回用户想要保存的文件的路径。

```php
use Native\Laravel\Dialog;

Dialog::new()
    ->title('保存文件')
    ->save();
```

### 配置文件对话框

#### 对话框标题

你可以使用 `title()` 方法设置对话框的标题。

```php
Dialog::new()
    ->title('选择文件')
    ->open();
```

#### 对话框按钮标签

你可以使用 `button()` 方法配置对话框按钮的标签。这是用户点击以确认其选择的按钮。

```php
Dialog::new()
    ->button('选择')
    ->open();
```

#### 对话框默认路径

你可以使用 `defaultPath()` 方法配置对话框的默认路径。这是对话框默认打开的路径，如果它存在的话。

```php
Dialog::new()
    ->defaultPath('/Users/username/Desktop')
    ->open();
```

#### 对话框文件过滤器

默认情况下，文件对话框将允许用户选择任何文件。你可以使用 `filter()` 方法限制用户可以选择的文件类型。一个对话框可以有多个过滤器。

`filter()` 方法的第一个参数是过滤器的名称，第二个参数是文件扩展名的数组。

```php
Dialog::new()
    ->filter('图像', ['jpg', 'png', 'gif'])
    ->filter('文档', ['pdf', 'docx'])
    ->open();
```

#### 允许选择多个文件

默认情况下，文件对话框将只允许用户选择一个文件。你可以使用 `multiple()` 方法更改此行为。这将导致 `open()` 方法返回文件路径数组，而不是单个文件路径字符串。

```php
$files = Dialog::new()
    ->multiple()
    ->open();
```

#### 显示隐藏文件

默认情况下，文件对话框不会显示隐藏文件（以点开头的文件）。你可以使用 `showHiddenFiles()` 方法更改此行为。

```php
Dialog::new()
    ->withHiddenFiles()
    ->open();
```

#### 解析符号链接

默认情况下，文件对话框将始终解析符号链接。这意味着如果你选择了一个符号链接，对话框将返回符号链接指向的文件或文件夹的路径。

你可以使用 `dontResolveSymlinks()` 方法更改此行为。

```php
Dialog::new()
    ->dontResolveSymlinks()
    ->open();
```

#### 以工作表形式打开对话框

默认情况下，所有 NativePHP 对话框都将作为独立的窗口打开，可以独立移动。

如果你想以“工作表”（附着在窗口上的对话框）的形式打开对话框，你可以使用 `asSheet()` 方法。`asSheet()` 方法的第一个参数是附着对话框的窗口的 ID。如果你没有指定窗口 ID，NativePHP 将使用当前聚焦窗口的 ID。

```php
Dialog::new()
    ->asSheet()
    ->open();
```

#### 打开文件夹

默认情况下，对话框打开文件或一组文件。

如果你想打开文件夹，你可以使用 `folders()` 方法。

```php
Dialog::new()
    ->folders()
    ->open();
```
