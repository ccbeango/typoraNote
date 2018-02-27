

# start-引导文件

## 源码

```php
namespace think;

// ThinkPHP 引导文件
// 1. 加载基础文件
require __DIR__ . '/base.php';

// 2. 执行应用
App::run()->send();
```

1. 加载基础文件，先看基础文件`base.php`
2. 执行应用App