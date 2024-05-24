# 配置

## 配置

`native:install` 命令会将一个配置文件发布到 `config/nativephp.php`。该文件包含了 NativePHP 的所有配置选项。

### 默认配置文件

```php
return [
    /**
     * 应用的版本号。
     * 用于判断应用是否需要更新。
     * 每次发布新版本的应用时，请增加此值。
     */
    'version' => env('NATIVEPHP_APP_VERSION', '1.0.0'),

    /**
     * 应用的唯一标识符。
     * 通常采用反向域名的形式。
     * 例如：com.nativephp.app
     */
    'app_id' => env('NATIVEPHP_APP_ID'),

    /**
     * 如果您的应用支持深度链接，可以在此指定使用的方案。
     * 这将用于从其他应用中打开您的应用。
     * 例如："nativephp"
     *
     * 这将允许您使用类似以下 URL 打开应用：
     * nativephp://some/path
     */
    'deeplink_scheme' => env('NATIVEPHP_DEEPLINK_SCHEME'),

    /**
     * 应用的作者。
     */
    'author' => env('NATIVEPHP_APP_AUTHOR'),

    /**
     * 应用的默认服务提供者。
     * 该提供者负责启动应用并配置任何全局热键、菜单、窗口等。
     */
    'provider' => \App\Providers\NativeAppServiceProvider::class,

    /**
     * 在应用打包为生产版本时，应从 .env 文件中移除的环境变量键列表。
     * 您可以使用通配符匹配多个键。
     */
    'cleanup_env_keys' => [
        'AWS_*',
        'GITHUB_*',
        'DO_SPACES_*',
        '*_SECRET',
        'NATIVEPHP_UPDATER_PATH',
        'NATIVEPHP_APPLE_ID',
        'NATIVEPHP_APPLE_ID_PASS',
        'NATIVEPHP_APPLE_TEAM_ID',
    ],

    /**
     * 在应用打包为生产版本前，应从最终应用中移除的文件和文件夹列表。
     * 您可以在此使用 glob 或通配符模式。
     */
    'cleanup_exclude_files' => [
        'content',
        'storage/app/framework/{sessions,testing,cache}',
        'storage/logs/laravel.log',
    ],

    /**
     * NativePHP 更新器配置。
     */
    'updater' => [
        /**
         * 更新器是否启用。
         * 请注意，更新器仅在应用打包为生产版本时有效。
         */
        'enabled' => env('NATIVEPHP_UPDATER_ENABLED', true),

        /**
         * 使用的更新器提供者。
         * 支持："github", "s3", "spaces"
         */
        'default' => env('NATIVEPHP_UPDATER_PROVIDER', 'spaces'),

        'providers' => [
            'github' => [
                'driver' => 'github',
                'repo' => env('GITHUB_REPO'),
                'owner' => env('GITHUB_OWNER'),
                'token' => env('GITHUB_TOKEN'),
                'vPrefixedTagName' => env('GITHUB_V_PREFIXED_TAG_NAME', true),
                'private' => env('GITHUB_PRIVATE', false),
                'channel' => env('GITHUB_CHANNEL', 'latest'),
                'releaseType' => env('GITHUB_RELEASE_TYPE', 'draft'),
            ],

            's3' => [
                'driver' => 's3',
                'key' => env('AWS_ACCESS_KEY_ID'),
                'secret' => env('AWS_SECRET_ACCESS_KEY'),
                'region' => env('AWS_DEFAULT_REGION'),
                'bucket' => env('AWS_BUCKET'),
                'endpoint' => env('AWS_ENDPOINT'),
                'path' => env('NATIVEPHP_UPDATER_PATH', null),
            ],

            'spaces' => [
                'driver' => 'spaces',
                'key' => env('DO_SPACES_KEY_ID'),
                'secret' => env('DO_SPACES_SECRET_ACCESS_KEY'),
                'name' => env('DO_SPACES_NAME'),
                'region' => env('DO_SPACES_REGION'),
                'path' => env('NATIVEPHP_UPDATER_PATH', null),
            ],
        ],
    ],
];
```

### 自定义 php.ini

当您的 NativePHP 应用启动时，您可能希望自定义 php.ini 指令，这些指令将用于您的应用。

您可以通过 `NativeAppServiceProvider` 上的 `phpIni()` 方法配置这些指令。该方法应返回要设置的 php.ini 指令数组。

```php
namespace App\Providers;

use Native\Laravel\Facades\Window;
use Native\Laravel\Contracts\ProvidesPhpIni;

class NativeAppServiceProvider implements ProvidesPhpIni
{
    /**
     * 在原生应用启动后执行一次。
     * 使用此方法打开窗口、注册全局快捷键等。
     */
    public function boot(): void
    {
        Window::open();
    }

    public function phpIni(): array
    {
        return [
            'memory_limit' => '512M',
            'display_errors' => '1',
            'error_reporting' => 'E_ALL',
            'max_execution_time' => '0',
            'max_input_time' => '0',
        ];
    }
}
```
