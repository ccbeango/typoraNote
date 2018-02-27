# 入口文件

## 源码

```php
// 定义应用目录
define('APP_PATH', __DIR__ . '/../application/');
// 加载框架引导文件
require __DIR__ . '/../thinkphp/start.php';
```

1. 入口文件十分简单，定义项目中将要用到常量，源码中之定义了`APP_PATH`，如果需要可自行定义其它常量，TP中的常量如不定义，将使用系统中的默认定义。
2. 加载项目引导文件

