# 调试

## 当事情出错时

构建原生应用是一个复杂的任务，涉及许多移动的部分。将会出现错误、崩溃和许多令人困惑的情况。

NativePHP 努力隐藏大部分复杂性，但有时您需要深入了解内部以找出真正发生的情况。

**请记住，NativePHP 是在许多外部开发者构建和维护的大量依赖和工具之上的一层相对较薄的层。**

这意味着虽然有些问题可以在 NativePHP 内部解决，但问题也很可能存在于其他地方。

### 层级

* 您的应用，基于 Laravel，使用您本地的 PHP 和 Node 安装。
* NativePHP 的开发工具（`native:serve` 和 `native:build`）管理 Electron/Tauri 的构建过程 - 这是创建您的应用包的过程。
* NativePHP 将适当版本的静态编译 PHP 二进制文件移动到您的应用包中 - 当您的应用启动时，是这个版本的 PHP 执行您的 PHP 代码，而不是您系统上的 PHP 版本。
* Electron 和 Tauri 使用一系列特定于平台的构建工具和依赖 - Electron 的生态系统主要是基于 JavaScript，而 Tauri 的主要是基于 Rust。其中大部分将隐藏在您的 `vendor` 目录中。
* 操作系统（OS）及其架构（arch） - 您不能为一种架构构建应用并将其分发到不同的 OS/arch。它不会工作。您必须根据您希望应用运行的 OS+arch 组合来构建应用。

虽然您不需要深入了解所有这些层级的工作原理和相互关系，但对正在发生的事情有一定的熟悉将帮助您找到问题的根本原因，并能够向正确的人提出有意义的票证。

### 进行一些挖掘

以下是一些调试提示：

#### 两到三个副本

请记住，当生成构建（开发或生产）时，您的整个 Laravel 应用会被复制到构建文件夹中。

开发构建副本存储在 `vendor` 中（神圣的嵌套，蝙蝠侠！）。

生产构建被打包在 `dist` 文件夹中。

这意味着至少有两个版本的代码可能会根据您正在做的事情运行：您在 IDE 中编辑的热代码（您的“开发环境”）和在开发构建或生产构建中运行应用时实际执行的包代码。

清楚地了解问题发生时的上下文将帮助您更快地解决问题。

#### 详细输出

在运行 `native:serve` 或 `native:build` 时使用 `-v`、`-vv` 或 `-vvv`，这将提供更多关于每个过程阶段发生情况的细节。

#### 检查日志

运行应用构建生成的日志存储在 `{appdata}/storage/logs/` 中。

在开发环境中运行 Artisan 命令生成的日志位于 `storage/logs/`（默认 Laravel）。

#### 逐步执行

尝试在运行时环境之外运行失败的步骤。减少抽象层以识别或排除特定于环境的复杂性。

#### 从头开始

**`dist`**

不要害怕删除构建并重新开始！您的应用根目录中的 `dist` 文件夹有时可能会进入异常状态，只需清除即可。

**AppData**

appdata 目录是存储应用的数据库、日志和其他特定于应用的项目的位置。这是存储应用运行所需的数据和文件的可靠位置，而不需要在用户的主目录或其他个人文件夹中造成混乱。

在测试生产构建时，appdata 目录将在您的机器上创建，允许您完全模拟最终用户的体验。

在某些情况下，您可能需要清除此文件夹，然后重新运行您的应用。

| 平台      | 位置                              |
| ------- | ------------------------------- |
| macOS   | \~/Library/Application Support  |
| Linux   | $XDG\_CONFIG\_HOME 或 \~/.config |
| Windows | %APPDATA%                       |

**数据库**

尝试完全刷新您的应用的生产数据库：

```shell
php artisan native:migrate:fresh
```

**进程**

确保没有遗留的进程。检查您的活动监视器/任务管理器，找到在构建失败后可能挂起的应用进程，并强制它们退出。

#### 检查您的应用和 PHP

在应用启动序列期间发生的 PHP 执行错误可能导致应用在启动之前崩溃。

例如，您的应用代码中的 500 错误可能会阻止主窗口显示，但会留下运行时 shell 进程运行。

尝试在标准浏览器中启动您的应用，以查看在访问其入口点 URL 时是否存在任何错误。例如，如果您使用 Laravel Herd，将您的应用开发环境移动到您的 Herd 根目录，并在您喜欢的浏览器中转到 `http://{your-app-folder-name}.test/`。

还要确保包中的 PHP 版本与您机器上安装的版本相同，即如果您在机器上运行 PHP 8.2，移动到 `dist` 文件夹的 PHP 二进制文件应该是您当前 OS+arch 的 PHP 8.2。

检查这一点也将证明可执行文件本身是稳定的：

**对于开发构建：**

macOS 和 Linux：

```shell
/path/to/your/app/vendor/nativephp/electron/resources/js/resources/php/php -v
```

Windows：

```
C:\path\to\your\app\vendor\nativephp\electron\resources\js\resources\php\php.exe -v
```

**对于生产构建：**

macOS：

```shell
/path/to/your/app/dist/{os+arch}/AppName/Contents/Resources/app.asar.unpacked/resources/php/php -v
```

Windows：

```
C:\path\to\your\app\dist\win-unpacked\resources\app.asar.unpacked\resources\php\php.exe -v
```

### 仍然卡住？

如果您发现了一个错误，请在 GitHub 上[打开一个问题](https://github.com/nativephp/laravel/issues/new)。

还有[讨论](https://github.com/orgs/NativePHP/discussions)和[Discord](https://discord.gg/X62tWNStZK) 用于实时聊天。

来加入我们吧！我们希望您成功。
