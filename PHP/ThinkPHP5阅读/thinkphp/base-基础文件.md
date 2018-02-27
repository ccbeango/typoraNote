# 基础文件

## 源码

###片段1

```php
define('THINK_VERSION', '5.0.14');
define('THINK_START_TIME', microtime(true));
define('THINK_START_MEM', memory_get_usage());
define('EXT', '.php');
define('DS', DIRECTORY_SEPARATOR); // php内置常量目录分隔符
defined('THINK_PATH') or define('THINK_PATH', __DIR__ . DS);  // __DIR__指向PHP当前脚本执行所在目录
define('LIB_PATH', THINK_PATH . 'library' . DS);
define('CORE_PATH', LIB_PATH . 'think' . DS);
define('TRAIT_PATH', LIB_PATH . 'traits' . DS);
// 获取应用目录application
defined('APP_PATH') or define('APP_PATH',dirname($_SERVER['SCRIPT_FILENAME']) . DS);
defined('ROOT_PATH') or define('ROOT_PATH', dirname(realpath(APP_PATH)) . DS); // 项目根目录
defined('EXTEND_PATH') or define('EXTEND_PATH', ROOT_PATH . 'extend' . DS); // extend目录
defined('VENDOR_PATH') or define('VENDOR_PATH', ROOT_PATH . 'vendor' . DS); // vender目录
defined('RUNTIME_PATH') or define('RUNTIME_PATH', ROOT_PATH . 'runtime' . DS);
defined('LOG_PATH') or define('LOG_PATH', RUNTIME_PATH . 'log' . DS);
defined('CACHE_PATH') or define('CACHE_PATH', RUNTIME_PATH . 'cache' . DS);
defined('TEMP_PATH') or define('TEMP_PATH', RUNTIME_PATH . 'temp' . DS);
defined('CONF_PATH') or define('CONF_PATH', APP_PATH); // 配置文件目录
defined('CONF_EXT') or define('CONF_EXT', EXT); // 配置文件后缀
defined('ENV_PREFIX') or define('ENV_PREFIX', 'PHP_'); // 环境变量的配置前缀

// 环境常量
define('IS_CLI', PHP_SAPI == 'cli' ? true : false); // 预定义常量cli
define('IS_WIN', strpos(PHP_OS, 'WIN') !== false); // 判断当前系统操作类型
```

####分析：

上述代码主要加载框架中一些常量：

* ` microtime(true)` 返回当前 Unix 时间戳的微秒数

语法： 

```Php
microtime(get_as_float);
参数 
  get_as_float
	可选。当设置为 TRUE 时，规定函数应该返回浮点数，否则返回字符串。默认为 FALSE。
```



* `memory_get_usage()`返回分配给php的内存量
* `DIRECTORY_SEPARATOR`PHP的内置常量，目录分隔符
* `__DIR__` 指向PHP当前脚本执行所在目录

###片段2

```php
// 载入Loader类
require CORE_PATH . 'Loader.php';
```

此文件分析在：Loader.php

### 片段3

```php
// 加载环境变量配置文件
if (is_file(ROOT_PATH . '.env')) {
    $env = parse_ini_file(ROOT_PATH . '.env', true);

    foreach ($env as $key => $val) {
        $name = ENV_PREFIX . strtoupper($key);

        if (is_array($val)) {
            foreach ($val as $k => $v) {
                $item = $name . '_' . strtoupper($k);
                putenv("$item=$v");
            }
        } else {
            putenv("$name=$val");
        }
    }
}
```

#### 分析：

如果在根目录下定义了.env文件,那么就会加载其中的环境变量

* `parse_ini_file(file,process_sections)` 解析一个配置文件，并以数组的形式返回其中的设置
  * `file`必需。规定要检查的 ini 文件;
  * `process_sections`可选。如果设置为 true，则返回一个多维数组，包括了配置文件中每一节的名称和设置。默认是 false
* `putenv(char)`向环境表中 添加或者修改 环境变量即配置系统环境变量；php服务器变量都在超全局变量`$_SERVER`中
  * string：指向环境变量的指针，其中环境变量必须以 `name=value` 的形式给出。如果环境表中没有`name`  这个环境变量，则添加该环境变量；如果环境表中已经有了name这个环境变量，则先删除之前的 value，再修改为新的 value。

### 片段4

```php
// 注册自动加载
\think\Loader::register();

// 注册错误和异常处理机制
\think\Error::register();

// 加载惯例配置文件
\think\Config::set(include THINK_PATH . 'convention' . EXT);
```

文件分析分别在：`Error.php`和` Config.php`