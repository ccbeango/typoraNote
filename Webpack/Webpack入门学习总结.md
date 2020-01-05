# Webpack入门学习总结

最近也多多少少接触一些前端框架，每当遇到webpack时都是一头雾水，打开配置能看懂的太少，对其没有整体的认识，想要修改配置就只能一个个去试或者去网上找相关配置片段复制粘贴，体验极差。

所以，找时间入门一下还是很有必要的。

## Webpack简介

Webpack是一个打包模块化 JavaScript 的工具，在 Webpack 里一切文件皆模块，通过 Loader 转换文件，通过 Plugin 注入钩子，最后输出由多个模块组合成的文件。Webpack 专注于构建模块化项目。

一切文件：JavaScript、CSS、SCSS、图片、模板，在 Webpack 眼中都是一个个模块，这样的好处是能清晰的描述出各个模块之间的依赖关系，以方便 Webpack 对模块进行组合和打包。 经过 Webpack 的处理，最终会输出浏览器能使用的静态资源。

Webpack的优点是：

- 专注于处理模块化的项目，能做到开箱即用一步到位；
- 通过 Plugin 扩展，完整好用又不失灵活；
- 使用场景不仅限于 Web 开发；
- 社区庞大活跃，经常引入紧跟时代发展的新特性，能为大多数场景找到已有的开源扩展；
- 良好的开发体验。

Webpack的缺点是只能用于采用模块化开发的项目。

## Webpack配置参数简介

首先来看一段简单的webpack配置：

```javascript
const path = require('path');
const ExtractTextPlugin = require('extract-text-webpack-plugin');

module.exports = {
  // JavaScript 执行入口文件
  entry: './main.js',
  output: {
    // 把所有依赖的模块合并输出到一个 bundle.js 文件
    filename: 'bundle.js',
    // 把输出文件都放到 dist 目录下
    path: path.resolve(__dirname, './dist'),
  },
  module: {
    rules: [
      {
        // 用正则去匹配要用该 loader 转换的 CSS 文件
        test: /\.css$/,
        // use: ['style-loader', 'css-loader?minimize'],
        use: ExtractTextPlugin.extract({
          // 转换 .css 文件需要使用的 Loader
          use: ['css-loader'],
        }),
      }
    ]
  },
  plugins: [
    new ExtractTextPlugin({
      // 从 .js 文件中提取出来的 .css 文件的名称
      filename: `[name]_[contenthash:8].css`,
    }),
  ]
};
```

各个参数解析：

* `entry`：Javascript执行入口文件，即输入配置。

* `output`：Javascript的输出文件配置。上述配置指的是，输出文件的文件名是`bundle.js`，路径在项目的根目录下的`dist`文件夹下。

* `module`：webpack下loader机制配置节点。Loader 可以看作具有文件转换功能的翻译员，配置里的 `module.rules` 数组配置了一组规则，告诉 Webpack 在遇到哪些文件时使用哪些 Loader 去加载和转换。

  * 上述注释掉的配置`use: ['style-loader', 'css-loader?minimize']`是指 Webpack 在遇到以 `.css` 结尾的文件时先使用 `css-loader` 读取 CSS 文件，再交给 `style-loader` 把 CSS 内容注入到 JavaScript 里。

  **配置Loader时要注意**：

  *  `use`属性的值需要是一个由 Loader 名称组成的数组，Loader 的执行顺序是由后到前的；

  * 每一个 Loader 都可以通过 URL querystring 的方式传入参数，例如 `css-loader?minimize` 中的 `minimize` 告诉 `css-loader` 要开启 CSS 压缩。

  * Loader传入属性的方式除了`querystring`外，还可以通过`Object`传入，所以配置可以修改为:

    ```javascript
    use: [
      'style-loader', 
      {
        loader:'css-loader',
        options:{
          minimize:true,
        }
      }
    ]
    ```

    还可以在源码中指定用什么Loader去处理文件，如：

    ```javascript
    require('style-loader!css-loader?minimize!./main.css');
    ```

    这样就能指定对 `./main.css` 这个文件先采用 css-loader 再采用 style-loader 转换。

* `plugins` 是用来扩展 Webpack 功能的，通过在构建流程里注入钩子实现。使用`plugins`可以配置需要使用的插件列表。它的值是一个数组，里面的每一项都是一个插件实例，在实例化一个组件时候，可以通过构造函数传入这个组件所支持的配置属性。

  `ExtractTextPlugin` 插件的作用是提取出 JavaScript 代码里的 CSS 到一个单独的文件。 对此你可以通过插件的 `filename` 属性，告诉插件输出的 CSS 文件名称是通过`[name]_[contenthash:8].css` 字符串模版生成的，里面的 `[name]` 代表文件名称， `[contenthash:8]` 代表根据文件内容算出的8位 hash值。

## Webpack的核心概念

Webpack的配置项很多，理解它的几个核心概念是很有必要的。

Webpack的几个核心概念如下：

* **Entry**：入口，Webpack执行构建的第一步将从`Entry`开始，可抽象成输入。
* **Module**：模块，在Webpack中一切皆为模块，**一个模块对应着一个文件**。Webpack会从配置的`entry`开始递归找出所有依赖的模块。
* **Chunk**：代码块，一个`Chunk`由多个模块组合而成，用于代码的合并与分割。
* **Loader**：模块转换器，用于把模块原内容按照需求转换成新内容。
* **Plugin**：扩展插件，在 Webpack 构建流程中的特定时机注入扩展逻辑来改变构建结果或做你想要的事情。
* **Output**：输出结果，在Webpack经过一系列处理并得到最终想要的代码后输出结果。



Webpack的流程：

Webpack启动后会从Entry里配置的Module（入口文件）开始递归解析Entry依赖的所有Module。每找到一个Module，就会根据配置的Loader去找出对应的转换规则，对Module进行转换后，再解析出当前Module所依赖的Module。这些模块会以Entry为单位进行分组，一个Entry和其所有依赖的Module被分到一个组，也就是一个Chunk。最后Webpack会把所有的Chunk转换成文件输出。在整个流程中，Webpack会在恰当的时机执行Plugin里定义的逻辑。

## Webpack的常用配置



### Context

Webpack 在寻找相对路径的文件时会以 `context` 为根目录，`context` 默认为执行启动 Webpack 时所在的当前工作目录。 如果想改变 `context` 的默认配置，则可以在配置文件里这样设置它：

```javascript
module.exports = {
  context: path.resolve(__dirname, 'app')
}
```

### Entry

配置模块的入口，从`enrty`开始搜寻及递归解析出所有入口依赖的模块。

`entry`选项是必填的，若不填写将会导致Webpack报错退出。

Entry 的路径和其依赖的模块的路径可能采用相对于 `context` 的路径来描述，`context` 会影响到这些相对路径所指向的真实文件。

#### Entry的类型

Entry 类型可以是以下三种中的一种或者相互组合：

| 类型   | 例子                                                         | 含义                                 |
| ------ | ------------------------------------------------------------ | ------------------------------------ |
| string | `./app/entry`                                                | 入口模块的文件路径，可以是相对路径。 |
| array  | `['./app/entry1', './app/entry2']`                           | 入口模块的文件路径，可以是相对路径。 |
| object | `{ a: './app/entry-a', b: ['./app/entry-b1', './app/entry-b2']}` | 配置多个入口，每个入口生成一个 Chunk |

如果是 `array` 类型，则搭配 `output.library` 配置项使用时，只有数组里的最后一个入口文件的模块会被导出。???

#### 命令

> 如果传入一个字符串或字符串数组，chunk 会被命名为 `main`。如果传入一个对象，则每个键(key)会是 chunk 的名称，该值描述了 chunk 的入口起点。

Webpack 会为每个生成的 Chunk 取一个名称，Chunk 的名称和 Entry 的配置有关：

- 如果 `entry` 是一个 `string` 或 `array`，就只会生成一个 Chunk，这时 Chunk 的名称是 `main`；
- 如果 `entry` 是一个 `object`，就可能会出现多个 Chunk，这时 Chunk 的名称是 `object` 键值对里键的名称。

#### 配置动态Entry

假如项目里有多个页面需要为每个页面的入口配置一个 Entry ，但这些页面的数量可能会不断增长，则这时 Entry 的配置会受到到其他因素的影响导致不能写成静态的值。其解决方法是把 Entry 设置成一个函数去动态返回上面所说的配置，代码如下：

```javascript
// 同步函数
entry: () => {
  return {
    a:'./pages/a',
    b:'./pages/b',
  }
};
// 异步函数
entry: () => {
  return new Promise((resolve)=>{
    resolve({
       a:'./pages/a',
       b:'./pages/b',
    });
  });
};
```

### Output

`output`配置如何输出最终想要的代码，它是一个`Object`。

下面是配置项的介绍。

#### filename

配置输出的文件名称，是`string`类型。

如果只有一个输出文件，可写成静态不变的：

```javascript
filename: 'bundle.js'
```

如果要输出多个Chunk，需借助模板和变量，Webpack会为每个Chunk取一个名称，可以根据Chunk的名称来区分输出的文件名：

```javascript
filename: '[name].js'
```

其中`[name]`代表内置的`name`变量去替换`[name]`，相当于一个字符串模板函数，每个要输出的Chunk都会通过这个函数去拼接输出的文件名称。

可用的模板字符串如下：

* `id`：Chunk的唯一表示，从0开始
* `name`：Chunk 的名称
* `hash`：Chunk的唯一标识的Hash值
* `chunkhash`：Chunk 内容的Hash值

其中`hash`和`chunkhash`的长度默认为20位，但可指定，如`[hash:8]`代表8位的Hash值。

#### chunkFilename

有点不明白是做什么用的。

`chunkFilename` 配置无入口的 Chunk 在输出时的文件名称。 `chunkFilename` 和上面的 `filename` 非常类似，但 `chunkFilename` 只用于指定在运行过程中生成的 Chunk 在输出时的文件名称。 

常见的会在运行时生成 Chunk 场景有在使用 CommonChunkPlugin、使用 `import('path/to/module')` 动态加载等时。

chunkFilename 支持和 filename 一致的内置变量。

#### path

配置输出文件存放的本地目录，必须是`string`类型的绝对路径。

```javascript
path: path.resolve(__dirname, 'dist_[hash]')
```

#### publicPath

对于按需加载(on-demand-load)或加载外部资源(external resources)（如图片、文件等）来说，output.publicPath 是很重要的选项。如果指定了一个错误的值，则在加载这些资源时会收到 404 错误。

`output.publicPath` 配置发布到线上资源的 URL 前缀，为string 类型。 默认值是空字符串 `''`，即使用相对路径。

内置变量只有一个`hash`，代表一个编译操作的Hash值。

如配置：

```javascript
filename:'[name]_[chunkhash:8].js'
publicPath: 'https://cdn.example.com/assets/'
```

这时发布到线上的 HTML 在引入 JavaScript 文件时就需要：

```html
<script src='https://cdn.example.com/assets/a_12345678.js'></script>
```

> 此选项指定在浏览器中所引用的「此输出目录对应的**公开 URL**」

```javascript
publicPath: "https://cdn.example.com/assets/", // CDN（总是 HTTPS 协议）
publicPath: "//cdn.example.com/assets/", // CDN (协议相同)
publicPath: "/assets/", // 相对于服务(server-relative)
publicPath: "assets/", // 相对于 HTML 页面
publicPath: "../assets/", // 相对于 HTML 页面
publicPath: "", // 相对于 HTML 页面（目录相同）
```

#### crossOriginLoading

只用于 `target` 是 web，使用了通过 script 标签的 JSONP 来按需加载 chunk。

Webpack 输出的部分代码块可能需要异步加载，而异步加载是通过 [JSONP](https://zh.wikipedia.org/wiki/JSONP) 方式实现的。 JSONP 的原理是动态地向 HTML 中插入一个 `<script src="url"></script> ` 标签去加载异步资源。 `output.crossOriginLoading` 则是用于配置这个异步插入的标签的 `crossorigin` 值。

script标签的crossorign属性可以取值如下：

* `false`： 禁用跨域加载（默认）
* `anonymous`: 不带凭据(credential)启用跨域加载，即在加载此脚本资源时不会带上用户的 Cookies
* `use-credentials`：带凭据(credential)启用跨域加载 with credentials，即在加载此脚本资源时会带上用户的 Cookies

通常用设置 crossorigin 来获取异步加载的脚本执行时的详细错误信息。

#### libraryTarget 和 library

当用 Webpack 去构建一个可以被其他模块导入使用的库时需要用到它们。

- `output.libraryTarget` 配置以何种方式导出库。
- `output.library` 配置导出库的名称。

两者通常搭配在一起使用。其中`libraryTarget`是字符串的枚举类型，入口起点的返回值将使用 `output.library` 中定义的值，配置如下：

##### var（默认）

编写的库将通过 `var` 被赋值给通过 `library` 指定名称的变量。 

假如配置了 `output.library='LibraryName'`，则输出和使用的代码如下：

```javascript
// Webpack 输出的代码
var LibraryName = lib_code;

// 使用库的方法
LibraryName.doSomething();
```

假如 `output.library` 为空，则将直接输出：

```javascript
lib_code
```

其中 `lib_code` 代指导出库的代码内容，是一个**有返回值的自执行函数**。

##### commonjs

编写的库将通过 CommonJS 规范导出。

假如配置了 `output.library='LibraryName'`，则输出和使用的代码如下：

```js
// Webpack 输出的代码
exports['LibraryName'] = lib_code;

// 使用库的方法
require('library-name-in-npm')['LibraryName'].doSomething();
```

其中 `library-name-in-npm` 是指模块发布到 Npm 代码仓库时的名称。

##### commonjs2

编写的库将通过 CommonJS2 规范导出，输出和使用的代码如下：

```js
// Webpack 输出的代码
module.exports = lib_code;

// 使用库的方法
require('library-name-in-npm').doSomething();
```

> CommonJS2 和 CommonJS 规范很相似，差别在于 CommonJS 只能用 `exports` 导出，而 CommonJS2 在 CommonJS 的基础上增加了 `module.exports` 的导出方式。
>
> 在 `output.libraryTarget` 为 `commonjs2` 时，配置 `output.library` 将没有意义。

##### this

编写的库将通过 `this` 被赋值给通过 `library` 指定的名称，`this`的含义取决于你，输出和使用的代码如下：

```js
// Webpack 输出的代码
this['LibraryName'] = lib_code;

// 使用库的方法
this.LibraryName.doSomething();
```

##### window

编写的库将通过 `window` 被赋值给通过 `library` 指定的名称，即把库挂载到 `window` 上，输出和使用的代码如下：

```javascript
// Webpack 输出的代码
window['LibraryName'] = lib_code;

// 使用库的方法
window.LibraryName.doSomething();
```

##### global

编写的库将通过 `global` 被赋值给通过 `library` 指定的名称，即把库挂载到 `global` 上，输出和使用的代码如下：

```js
// Webpack 输出的代码
global['LibraryName'] = lib_code;

// 使用库的方法
global.LibraryName.doSomething();
```

##### libraryExport

`output.libraryExport` 配置要导出的模块中哪些子模块需要被导出。

### Module

配置如何处理模块。

#### 配置Loader

`rules` 配置模块的读取和解析规则，通常用来配置 Loader。其类型是一个数组，数组里每一项都描述了如何去处理部分文件。

 配置一项 `rules` 时大致通过以下方式：

1. 条件匹配：通过 `test` 、 `include` 、 `exclude` 三个配置项来命中 Loader 要应用规则的文件。
2. 应用规则：对选中后的文件通过 `use` 配置项来应用 Loader，可以只应用一个 Loader 或者按照从后往前的顺序应用一组 Loader，同时还可以分别给 Loader 传入参数。
3. 重置顺序：一组 Loader 的执行顺序默认是从右到左执行，通过 `enforce` 选项可以让其中一个 Loader 的执行顺序放到最前或者最后。

例子：

```javascript
module: {
  rules: [
    {
      // 命中 JavaScript 文件
      test: /\.js$/,
      // 用 babel-loader 转换 JavaScript 文件
      // ?cacheDirectory 表示传给 babel-loader 的参数，用于缓存 babel 编译结果加快重新编译速度
      use: ['babel-loader?cacheDirectory'],
      // 只命中src目录里的js文件，加快 Webpack 搜索速度
      include: path.resolve(__dirname, 'src')
    },
    {
      // 命中 SCSS 文件
      test: /\.scss$/,
      // 使用一组 Loader 去处理 SCSS 文件。
      // 处理顺序为从后到前，即先交给 sass-loader 处理，再把结果交给 css-loader 最后再给 style-loader。
      use: ['style-loader', 'css-loader', 'sass-loader'],
      // 排除 node_modules 目录下的文件
      exclude: path.resolve(__dirname, 'node_modules'),
    },
    {
      // 对非文本文件采用 file-loader 加载
      test: /\.(gif|png|jpe?g|eot|woff|ttf|svg|pdf)$/,
      use: ['file-loader'],
    },
  ]
}
```

在 Loader 需要传入很多参数时，你还可以通过一个 Object 来描述，例如在上面的 babel-loader 配置中有如下代码：

```js
use: [
  {
    loader:'babel-loader',
    options:{
      cacheDirectory:true,
    },
    // enforce:'post' 的含义是把该 Loader 的执行顺序放到最后
    // enforce 的值还可以是 pre，代表把 Loader 的执行顺序放到最前面
    enforce:'post'
  },
  // 省略其它 Loader
]
```

上面的例子中 `test include exclude` 这三个命中文件的配置项只传入了一个字符串或正则，其实它们还都支持数组类型，使用如下：

```javascript
{
  test:[
    /\.jsx?$/,
    /\.tsx?$/
  ],
  include:[
    path.resolve(__dirname, 'src'),
    path.resolve(__dirname, 'tests'),
  ],
  exclude:[
    path.resolve(__dirname, 'node_modules'),
    path.resolve(__dirname, 'bower_modules'),
  ]
}
```

数组中每项之间是**或**的关系，即文件路径符合数组中的任何一个条件就会被命中。

#### noParse

`noParse` 配置项可以让 Webpack 忽略对部分没采用模块化的文件的递归解析和处理，这样做的好处是能提高构建性能。 原因是一些库例如 jQuery 、ChartJS 它们庞大又没有采用模块化标准，让 Webpack 去解析这些文件耗时又没有意义。

`noParse` 是可选配置项，类型需要是 `RegExp`、`[RegExp]`、`function` 其中一个。

例如想要忽略掉 jQuery 、ChartJS，可以使用如下代码：

```javascript
// 使用正则表达式
noParse: /jquery|chartjs/

// 使用函数，从 Webpack 3.0.0 开始支持
noParse: (content)=> {
  // content 代表一个模块的文件路径
  // 返回 true or false
  return /jquery|chartjs/.test(content);
}
```

> 注意被忽略掉的文件里不应该包含 `import` 、 `require` 、 `define` 等模块化语句，不然会导致构建出的代码中包含无法在浏览器环境下执行的模块化语句。

#### parse

因为 Webpack 是以模块化的 JavaScript 文件为入口，所以内置了对模块化 JavaScript 的解析功能，支持 AMD、CommonJS、SystemJS、ES6。 `parser` 属性可以更细粒度的配置哪些模块语法要解析哪些不解析，和 `noParse` 配置项的区别在于 `parser` 可以精确到语法层面， 而 `noParse` 只能控制哪些文件不被解析。 `parser` 使用如下：

```js
module: {
  rules: [
    {
      test: /\.js$/,
      use: ['babel-loader'],
      parser: {
          amd: false, // 禁用 AMD
          commonjs: false, // 禁用 CommonJS
          system: false, // 禁用 SystemJS
          harmony: false, // 禁用 ES6 import/export
          requireInclude: false, // 禁用 require.include
          requireEnsure: false, // 禁用 require.ensure
          requireContext: false, // 禁用 require.context
          browserify: false, // 禁用 browserify
          requireJs: false, // 禁用 requirejs
          node: false, // 禁用 __dirname, __filename, module, require.extensions, require.main 等。
  		  node: {...} // 在模块级别(module level)上重新配置 node 层(layer)
      }
    },
  ]
}
```

### Resolve

Webpack 在启动后会从配置的入口模块出发找出所有依赖的模块，**Resolve 配置 Webpack 如何寻找模块所对应的文件**。 

Webpack 内置 JavaScript 模块化语法解析功能，默认会采用模块化标准里约定好的规则去寻找，但你也可以根据自己的需要修改默认的规则。

#### alias

`resolve.alias` 配置项通过别名来把原导入路径映射成一个新的导入路径。例如使用以下配置：

```javascript
// Webpack alias 配置
resolve:{
  alias:{
    components: './src/components/'
  }
}
```

当你通过 `import Button from 'components/button'` 导入时，实际上被 `alias` 等价替换成了 `import Button from './src/components/button'`。

以上 alias 配置的含义是把导入语句里的 `components` 关键字替换成 `./src/components/`。

使用`$`符号来缩小范围到只命中以关键字结尾的导入语：

```javascript
resolve:{
  alias:{
    'react$': '/path/to/react.min.js'
  }
}
```

`react$` 只会命中以 `react` 结尾的导入语句，即只会把 `import 'react'` 关键字替换成 `import '/path/to/react.min.js'`。

#### mainFields

有一些第三方模块会针对不同环境提供几份代码。 例如分别提供采用 ES5 和 ES6 的2份代码，这2份代码的位置写在 `package.json` 文件里，如下：

```json
{
  "jsnext:main": "es/index.js",// 采用 ES6 语法的代码入口文件
  "main": "lib/index.js" // 采用 ES5 语法的代码入口文件
}
```

Webpack 会根据 `mainFields` 的配置去决定优先采用那份代码，`mainFields` 默认如下：

```js
mainFields: ['browser', 'main']
```

Webpack 会按照数组里的顺序去`package.json` 文件里寻找，只会使用找到的第一个。

假如你想优先采用 ES6 的那份代码，可以这样配置：

```js
mainFields: ['jsnext:main', 'browser', 'main']
```

#### extensions

在导入语句没带文件后缀时，Webpack 会自动带上后缀后去尝试访问文件是否存在。 `resolve.extensions` 用于配置在尝试过程中用到的后缀列表，默认是：

```js
extensions: ['.js', '.json']
```

也就是说当遇到 `require('./data')` 这样的导入语句时，Webpack 会先去寻找 `./data.js` 文件，如果该文件不存在就去寻找 `./data.json` 文件， 如果还是找不到就报错。

假如你想让 Webpack 优先使用目录下的 TypeScript 文件，可以这样配置：

```js
extensions: ['.ts', '.js', '.json']
```

#### modules

`resolve.modules` 配置 Webpack 去哪些目录下寻找第三方模块，默认是只会去 `node_modules` 目录下寻找。 有时你的项目里会有一些模块会大量被其它模块依赖和导入，由于其它模块的位置分布不定，针对不同的文件都要去计算被导入模块文件的相对路径， 这个路径有时候会很长，就像这样 `import '../../../components/button'` 这时你可以利用 `modules` 配置项优化，假如那些被大量导入的模块都在 `./src/components` 目录下，把 `modules` 配置成

```js
modules:['./src/components','node_modules']
```

后，可以简单通过 `import 'button'` 导入。

#### descriptionFiles

`resolve.descriptionFiles` 配置描述第三方模块的文件名称，也就是 `package.json` 文件。默认如下：

```js
descriptionFiles: ['package.json']
```

#### enforceExtension

`resolve.enforceExtension` 如果配置为 `true` 所有导入语句都必须要带文件后缀， 例如开启前 `import './foo'` 能正常工作，开启后就必须写成 `import './foo.js'`。

#### enforceModuleExtension

`enforceModuleExtension` 和 `enforceExtension` 作用类似，但 `enforceModuleExtension` 只对 `node_modules` 下的模块生效。 `enforceModuleExtension` 通常搭配 `enforceExtension` 使用，在 `enforceExtension:true` 时，因为安装的第三方模块中大多数导入语句没带文件后缀， 所以这时通过配置 `enforceModuleExtension:false` 来兼容第三方模块。

### Plugins

Plugin用于扩展Webpack的功能，且配置方法简单。

`plugins`配置项接受一个数组，数组中每项都是一个Plugin实例，实例需要的参数通过构造函数传入。

```javascript
const CommonsChunkPlugin = require('webpack/lib/optimize/CommonsChunkPlugin');

module.exports = {
  plugins: [
    // 所有页面都会用到的公共代码提取到 common 代码块中
    new CommonsChunkPlugin({
      name: 'common',
      chunks: ['a', 'b']
    }),
  ]
};
```

使用 Plugin 的难点在于掌握 Plugin 本身提供的配置项，而不是如何在 Webpack 中接入 Plugin。

### DevServer

DevServer为来提高开发阶段的效率。配置Devserver，可以在配置文件里通过配置`devServer`传入参数，还可以通过命令行传入参数。

只有通过DevServer去启动Webpack时配置文件里的`devServer`才会生效，因为这些参数所对应的功能都是DevServer提供的，Webpack本身并不认识`devServer`配置项。

#### hot

提到的模块热替换功能。 DevServer 默认的行为是在发现源代码被更新后会通过自动刷新整个页面来做到实时预览，开启模块热替换功能后将在不刷新整个页面的情况下通过用新模块替换老模块来做到实时预览。

#### inline

DevServer 的实时预览功能依赖一个注入到页面里的代理客户端去接受来自 DevServer 的命令和负责刷新网页的工作。 `devServer.inline` 用于配置是否自动注入这个代理客户端到将运行在页面里的 Chunk 里去，默认是会自动注入。 DevServer 会根据你是否开启 `inline` 来调整它的自动刷新策略：

- 如果开启 `inline`，DevServer 会在构建完变化后的代码时通过代理客户端控制网页刷新。
- 如果关闭 `inline`，DevServer 将无法直接控制要开发的网页。这时它会通过 iframe 的方式去运行要开发的网页，当构建完变化后的代码时通过刷新 iframe 来实现实时预览。 但这时你需要去 `http://localhost:8080/webpack-dev-server/` 实时预览你的网页了。

如果你想使用 DevServer 去自动刷新网页实现实时预览，最方便的方法是直接开启 `inline`。

#### historyApiFallback

`devServer.historyApiFallback` 用于方便的开发使用了 [HTML5 History API](https://developer.mozilla.org/en-US/docs/Web/API/History) 的单页应用。 这类单页应用要求服务器在针对任何命中的路由时都返回一个对应的 HTML 文件，例如在访问 `http://localhost/user` 和 `http://localhost/home` 时都返回 `index.html` 文件， 浏览器端的 JavaScript 代码会从 URL 里解析出当前页面的状态，显示出对应的界面。

配置 `historyApiFallback` 最简单的做法是：

```js
historyApiFallback: true
```

这会导致任何请求都会返回 `index.html` 文件，这只能用于只有一个 HTML 文件的应用。

如果你的应用由多个单页应用组成，这就需要 DevServer 根据不同的请求来返回不同的 HTML 文件，配置如下：

```js
historyApiFallback: {
  // 使用正则匹配命中路由
  rewrites: [
    // /user 开头的都返回 user.html
    { from: /^\/user/, to: '/user.html' },
    { from: /^\/game/, to: '/game.html' },
    // 其它的都返回 index.html
    { from: /./, to: '/index.html' },
  ]
}
```

#### contentBase

`devServer.contentBase` 配置 DevServer HTTP 服务器的文件根目录。 默认情况下为当前执行目录，通常是项目根目录，所有一般情况下你不必设置它，除非你有额外的文件需要被 DevServer 服务。 例如你想把项目根目录下的 `public` 目录设置成 DevServer 服务器的文件根目录，你可以这样配置：

```js
devServer:{
  contentBase: path.join(__dirname, 'public')
}
```

这里需要指出可能会让你疑惑的地方，DevServer 服务器通过 HTTP 服务暴露出的文件分为两类：

- 暴露本地文件。
- 暴露 Webpack 构建出的结果，由于构建出的结果交给了 DevServer，所以你在使用了 DevServer 时在本地找不到构建出的文件。

`contentBase` 只能用来配置暴露本地文件的规则，你可以通过 `contentBase:false` 来关闭暴露本地文件。

#### headers

`devServer.headers` 配置项可以在 HTTP 响应中注入一些 HTTP 响应头，使用如下：

```js
devServer:{
  headers: {
    'X-foo':'bar'
  }
}
```

#### port

`devServer.port` 配置项用于配置 DevServer 服务监听的端口，默认使用 8080 端口。 如果 8080 端口已经被其它程序占有就使用 8081，如果 8081 还是被占用就使用 8082，以此类推。

#### allowedHosts

 配置一个白名单列表，只有 HTTP 请求的 HOST 在列表里才正常返回，使用如下：

```js
allowedHosts: [
  // 匹配单个域名
  'host.com',
  'sub.host.com',
  // host2.com 和所有的子域名 *.host2.com 都将匹配
  '.host2.com'
]
```

#### disableHostCheck

`devServer.disableHostCheck` 配置项用于配置是否关闭用于 DNS 重绑定的 HTTP 请求的 HOST 检查。 DevServer 默认只接受来自本地的请求，关闭后可以接受来自任何 HOST 的请求。 它通常用于搭配 `--host 0.0.0.0` 使用，因为你想要其它设备访问你本地的服务，但访问时是直接通过 IP 地址访问而不是 HOST 访问，所以需要关闭 HOST 检查。

#### https

DevServer 默认使用 HTTP 协议服务，它也能通过 HTTPS 协议服务。 有些情况下你必须使用 HTTPS，例如 HTTP2 和 Service Worker 就必须运行在 HTTPS 之上。 要切换成 HTTPS 服务，最简单的方式是：

```js
devServer:{
  https: true
}
```

DevServer 会自动的为你生成一份 HTTPS 证书。

如果你想用自己的证书可以这样配置：

```js
devServer:{
  https: {
    key: fs.readFileSync('path/to/server.key'),
    cert: fs.readFileSync('path/to/server.crt'),
    ca: fs.readFileSync('path/to/ca.pem')
  }
}
```

#### clientLogLevel

`devServer.clientLogLevel` 配置在客户端的日志等级，这会影响到你在浏览器开发者工具控制台里看到的日志内容。 `clientLogLevel` 是枚举类型，可取如下之一的值 `none | error | warning | info`。 默认为 `info` 级别，即输出所有类型的日志，设置成 `none` 可以不输出任何日志。

#### compress

`evServer.compress` 配置是否启用 gzip 压缩。`boolean` 为类型，默认为 `false`。

#### open

`devServer.open` 用于在 DevServer 启动且第一次构建完时自动用你系统上默认的浏览器去打开要开发的网页。 同时还提供 `devServer.openPage` 配置项用于打开指定 URL 的网页。

### 其他配置项

#### Target

`target` 配置项可以让 Webpack 构建出针对不同运行环境的代码。

| target值            | 描述                                              |
| ------------------- | ------------------------------------------------- |
| `web`               | 针对浏览器 **(默认)**，所有代码都集中在一个文件里 |
| `node`              | 针对 Node.js，使用 `require` 语句加载 Chunk 代码  |
| `async-node`        | 针对 Node.js，异步加载 Chunk 代码                 |
| `webworker`         | 针对 WebWorker                                    |
| `electron-main`     | 针对 Electron 主线程                              |
| `electron-renderer` | 针对 Electron 渲染线程                            |

例如当你设置 `target:'node'` 时，源代码中导入 Node.js 原生模块的语句 `require('fs')` 将会被保留，`fs` 模块的内容不会打包进 Chunk 里。

#### Devtool

 配置 Webpack 如何生成 Source Map，默认值是 `false` 即不生成 Source Map，想为构建出的代码生成 Source Map 以方便调试，可以这样配置：

```js
module.export = {
  devtool: 'source-map'
}
```

#### Watch 和 WatchOptions

前面介绍过 Webpack 的监听模式，它支持监听文件更新，在文件发生变化时重新编译。在使用 Webpack 时监听模式默认是关闭的，想打开需要如下配置：

```js
module.export = {
  watch: true
}
```

在使用 DevServer 时，监听模式默认是开启的。

除此之外，Webpack 还提供了 `watchOptions` 配置项去更灵活的控制监听模式，使用如下：

```js
module.export = {
  // 只有在开启监听模式时，watchOptions 才有意义
  // 默认为 false，也就是不开启
  watch: true,
  // 监听模式运行时的参数
  // 在开启监听模式时，才有意义
  watchOptions: {
    // 不监听的文件或文件夹，支持正则匹配
    // 默认为空
    ignored: /node_modules/,
    // 监听到变化发生后会等300ms再去执行动作，防止文件更新太快导致重新编译频率太高
    // 默认为 300ms  
    aggregateTimeout: 300,
    // 判断文件是否发生变化是通过不停的去询问系统指定文件有没有变化实现的
    // 默认每隔1000毫秒询问一次
    poll: 1000
  }
}
```

#### Externals

Externals 用来告诉 Webpack 要构建的代码中使用了哪些不用被打包的模块，也就是说这些模版是外部环境提供的，Webpack 在打包时可以忽略它们。

有些 JavaScript 运行环境可能内置了一些全局变量或者模块，例如在你的 HTML HEAD 标签里通过以下代码：

```html
<script src="path/to/jquery.js"></script>
```

引入 jQuery 后，全局变量 `jQuery` 就会被注入到网页的 JavaScript 运行环境里。

如果想在使用模块化的源代码里导入和使用 jQuery，可能需要这样：

```js
import $ from 'jquery';
$('.my-element');
```

构建后你会发现输出的 Chunk 里包含的 jQuery 库的内容，这导致 jQuery 库出现了2次，浪费加载流量，最好是 Chunk 里不会包含 jQuery 库的内容。

Externals 配置项就是为了解决这个问题。

通过 `externals` 可以告诉 Webpack JavaScript 运行环境已经内置了那些全局变量，针对这些全局变量不用打包进代码中而是直接使用全局变量。 要解决以上问题，可以这样配置 `externals`：

```js
module.export = {
  externals: {
    // 把导入语句里的 jquery 替换成运行环境里的全局变量 jQuery
    jquery: 'jQuery'
  }
}
```

#### ResolveLoader

ResolveLoader 用来告诉 Webpack 如何去寻找 Loader，因为在使用 Loader 时是通过其包名称去引用的， Webpack 需要根据配置的 Loader 包名去找到 Loader 的实际代码，以调用 Loader 去处理源文件。

ResolveLoader 的默认配置如下：

```js
module.exports = {
  resolveLoader:{
    // 去哪个目录下寻找 Loader
    modules: ['node_modules'],
    // 入口文件的后缀
    extensions: ['.js', '.json'],
    // 指明入口文件位置的字段
    mainFields: ['loader', 'main']
  }
}
```

该配置项常用于加载本地的 Loader。

### 整体配置结构

```js
const path = require('path');

module.exports = {
  // entry 表示 入口，Webpack 执行构建的第一步将从 Entry 开始，可抽象成输入。
  // 类型可以是 string | object | array   
  entry: './app/entry', // 只有1个入口，入口只有1个文件
  entry: ['./app/entry1', './app/entry2'], // 只有1个入口，入口有2个文件
  entry: { // 有2个入口
    a: './app/entry-a',
    b: ['./app/entry-b1', './app/entry-b2']
  },

  // 如何输出结果：在 Webpack 经过一系列处理后，如何输出最终想要的代码。
  output: {
    // 输出文件存放的目录，必须是 string 类型的绝对路径。
    path: path.resolve(__dirname, 'dist'),

    // 输出文件的名称
    filename: 'bundle.js', // 完整的名称
    filename: '[name].js', // 当配置了多个 entry 时，通过名称模版为不同的 entry 生成不同的文件名称
    filename: '[chunkhash].js', // 根据文件内容 hash 值生成文件名称，用于浏览器长时间缓存文件

    // 发布到线上的所有资源的 URL 前缀，string 类型
    publicPath: '/assets/', // 放到指定目录下
    publicPath: '', // 放到根目录下
    publicPath: 'https://cdn.example.com/', // 放到 CDN 上去

    // 导出库的名称，string 类型
    // 不填它时，默认输出格式是匿名的立即执行函数
    library: 'MyLibrary',

    // 导出库的类型，枚举类型，默认是 var
    // 可以是 umd | umd2 | commonjs2 | commonjs | amd | this | var | assign | window | global | jsonp ，
    libraryTarget: 'umd', 

    // 是否包含有用的文件路径信息到生成的代码里去，boolean 类型
    pathinfo: true, 

    // 附加 Chunk 的文件名称
    chunkFilename: '[id].js',
    chunkFilename: '[chunkhash].js',

    // JSONP 异步加载资源时的回调函数名称，需要和服务端搭配使用
    jsonpFunction: 'myWebpackJsonp',

    // 生成的 Source Map 文件名称
    sourceMapFilename: '[file].map',

    // 浏览器开发者工具里显示的源码模块名称
    devtoolModuleFilenameTemplate: 'webpack:///[resource-path]',

    // 异步加载跨域的资源时使用的方式
    crossOriginLoading: 'use-credentials',
    crossOriginLoading: 'anonymous',
    crossOriginLoading: false,
  },

  // 配置模块相关
  module: {
    rules: [ // 配置 Loader
      {  
        test: /\.jsx?$/, // 正则匹配命中要使用 Loader 的文件
        include: [ // 只会命中这里面的文件
          path.resolve(__dirname, 'app')
        ],
        exclude: [ // 忽略这里面的文件
          path.resolve(__dirname, 'app/demo-files')
        ],
        use: [ // 使用那些 Loader，有先后次序，从后往前执行
          'style-loader', // 直接使用 Loader 的名称
          {
            loader: 'css-loader',      
            options: { // 给 html-loader 传一些参数
            }
          }
        ]
      },
    ],
    noParse: [ // 不用解析和处理的模块
      /special-library\.js$/  // 用正则匹配
    ],
  },

  // 配置插件
  plugins: [
  ],

  // 配置寻找模块的规则
  resolve: { 
    modules: [ // 寻找模块的根目录，array 类型，默认以 node_modules 为根目录
      'node_modules',
      path.resolve(__dirname, 'app')
    ],
    extensions: ['.js', '.json', '.jsx', '.css'], // 模块的后缀名
    alias: { // 模块别名配置，用于映射模块
       // 把 'module' 映射 'new-module'，同样的 'module/path/file' 也会被映射成 'new-module/path/file'
      'module': 'new-module',
      // 使用结尾符号 $ 后，把 'only-module' 映射成 'new-module'，
      // 但是不像上面的，'module/path/file' 不会被映射成 'new-module/path/file'
      'only-module$': 'new-module', 
    },
    alias: [ // alias 还支持使用数组来更详细的配置
      {
        name: 'module', // 老的模块
        alias: 'new-module', // 新的模块
        // 是否是只映射模块，如果是 true 只有 'module' 会被映射，如果是 false 'module/inner/path' 也会被映射
        onlyModule: true, 
      }
    ],
    symlinks: true, // 是否跟随文件软链接去搜寻模块的路径
    descriptionFiles: ['package.json'], // 模块的描述文件
    mainFields: ['main'], // 模块的描述文件里的描述入口的文件的字段名称
    enforceExtension: false, // 是否强制导入语句必须要写明文件后缀
  },

  // 输出文件性能检查配置
  performance: { 
    hints: 'warning', // 有性能问题时输出警告
    hints: 'error', // 有性能问题时输出错误
    hints: false, // 关闭性能检查
    maxAssetSize: 200000, // 最大文件大小 (单位 bytes)
    maxEntrypointSize: 400000, // 最大入口文件大小 (单位 bytes)
    assetFilter: function(assetFilename) { // 过滤要检查的文件
      return assetFilename.endsWith('.css') || assetFilename.endsWith('.js');
    }
  },

  devtool: 'source-map', // 配置 source-map 类型

  context: __dirname, // Webpack 使用的根目录，string 类型必须是绝对路径

  // 配置输出代码的运行环境
  target: 'web', // 浏览器，默认
  target: 'webworker', // WebWorker
  target: 'node', // Node.js，使用 `require` 语句加载 Chunk 代码
  target: 'async-node', // Node.js，异步加载 Chunk 代码
  target: 'node-webkit', // nw.js
  target: 'electron-main', // electron, 主线程
  target: 'electron-renderer', // electron, 渲染线程

  externals: { // 使用来自 JavaScript 运行环境提供的全局变量
    jquery: 'jQuery'
  },

  stats: { // 控制台输出日志控制
    assets: true,
    colors: true,
    errors: true,
    errorDetails: true,
    hash: true,
  },

  devServer: { // DevServer 相关的配置
    proxy: { // 代理到后端服务接口
      '/api': 'http://localhost:3000'
    },
    contentBase: path.join(__dirname, 'public'), // 配置 DevServer HTTP 服务器的文件根目录
    compress: true, // 是否开启 gzip 压缩
    historyApiFallback: true, // 是否开发 HTML5 History API 网页
    hot: true, // 是否开启模块热替换功能
    https: false, // 是否开启 HTTPS 模式
    },

    profile: true, // 是否捕捉 Webpack 构建的性能信息，用于分析什么原因导致构建性能不佳

    cache: false, // 是否启用缓存提升构建速度

    watch: true, // 是否开始
    watchOptions: { // 监听模式选项
    // 不监听的文件或文件夹，支持正则匹配。默认为空
    ignored: /node_modules/,
    // 监听到变化发生后会等300ms再去执行动作，防止文件更新太快导致重新编译频率太高
    // 默认为300ms 
    aggregateTimeout: 300,
    // 判断文件是否发生变化是不停的去询问系统指定文件有没有变化，默认每隔1000毫秒询问一次
    poll: 1000
  },
}
```

### 多种配置类型

除了通过导出一个 Object 来描述 Webpack 所需的配置外，还有其它更灵活的方式，以简化不同场景的配置。 下面来一一介绍它们。

#### 导出一个 Function

在大多数时候你需要从同一份源代码中构建出多份代码，例如一份用于开发时，一份用于发布到线上。

如果采用导出一个 Object 来描述 Webpack 所需的配置的方法，需要写两个文件。 一个用于开发环境，一个用于线上环境。再在启动时通过 `webpack --config webpack.config.js` 指定使用哪个配置文件。

采用导出一个 Function 的方式，能通过 JavaScript 灵活的控制配置，做到只用写一个配置文件就能完成以上要求。

导出一个 Function 的使用方式如下：

```js
const path = require('path');
const UglifyJsPlugin = require('webpack/lib/optimize/UglifyJsPlugin');

module.exports = function (env = {}, argv) {
  const plugins = [];

  const isProduction = env['production'];

  // 在生成环境才压缩
  if (isProduction) {
    plugins.push(
      // 压缩输出的 JS 代码
      new UglifyJsPlugin()
    )
  }

  return {
    plugins: plugins,
    // 在生成环境不输出 Source Map
    devtool: isProduction ? undefined : 'source-map',
  };
}
```

在运行 Webpack 时，会给这个函数传入2个参数，分别是：

1. `env`：当前运行时的 Webpack 专属环境变量，`env` 是一个 Object。读取时直接访问 Object 的属性，设置它需要在启动 Webpack 时带上参数。例如启动命令是 `webpack --env.production --env.bao=foo`时，则 `env` 的值是 `{"production":"true","bao":"foo"}`。
2. `argv`：代表在启动 Webpack 时所有通过命令行传入的参数，例如 `--config`、`--env`、`--devtool`，可以通过 `webpack -h` 列出所有 Webpack 支持的命令行参数。

就以上配置文件而言，在开发时执行命令 `webpack` 构建出方便调试的代码，在需要构建出发布到线上的代码时执行 `webpack --env.production` 构建出压缩的代码。

> 本实例 [提供项目完整代码](http://webpack.wuhaolin.cn/2-9多种配置类型.zip)

#### 导出一个返回 Promise 的函数

在有些情况下你不能以同步的方式返回一个描述配置的 Object，Webpack 还支持导出一个返回 Promise 的函数，使用如下：

```js
module.exports = function(env = {}, argv) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({
        // ...
      })
    }, 5000)
  })
}
```

#### 导出多份配置

除了只导出一份配置外，Webpack 还支持导出一个数组，数组中可以包含每份配置，并且每份配置都会执行一遍构建。

> 注意本特性从 Webpack 3.1.0 版本才开始支持。

使用如下：

```js
module.exports = [
  // 采用 Object 描述的一份配置
  {
    // ...
  },
  // 采用函数描述的一份配置
  function() {
    return {
      // ...
    }
  },
  // 采用异步函数描述的一份配置
  function() {
    return Promise();
  }
]
```

以上配置会导致 Webpack 针对这三份配置执行三次不同的构建。

这特别适合于用 Webpack 构建一个要上传到 Npm 仓库的库，因为库中可能需要包含多种模块化格式的代码，例如 CommonJS、UMD。

### 总结

从前面的配置看来选项很多，Webpack 内置了很多功能。不必都记住它们，只需要大概明白 Webpack 原理和核心概念去判断选项大致属于哪个大模块下，再去查详细的使用文档。

通常你可用如下经验去判断如何配置 Webpack：

- 想让源文件加入到构建流程中去被 Webpack 控制，配置 `entry`。
- 想自定义输出文件的位置和名称，配置 `output`。
- 想自定义寻找依赖模块时的策略，配置 `resolve`。
- 想自定义解析和转换文件的策略，配置 `module`，通常是配置 `module.rules` 里的 Loader。
- 其它的大部分需求可能要通过 Plugin 去实现，配置 `plugin`。

