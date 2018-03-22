# React零碎点总结

1.`create-ract-app`可创建项目

2.`npm run eject`弹出配置文件，可以自定义配置 webpack配置

3.`axios`  Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

使用时，需要在package.json文件中添加后端的服务器代理地址：

```
"proxy": "http://localhost:9093";
```

安装：

```
npm install axios —-save
```

4.`antd-mobile` 是样式插件，实现按需加载，需要在package.json中的babel的plugins下添加：

```json
[
  "import",
  {

    "libraryName": "antd-mobile",

    "style": "css"

  }
],
```

安装：

```
npm install antd-mobile —-save
```

5.`babel-plugin-transform-decorators-legacy`  装饰器语法支，安装之后需要在babel的plugins下添加：

```
"plugins": ["transform-decorators-legacy"]
```

安装：

```
npm install babel-plugin-transform-decorators-legacy —-save
```

```json
"babel": {
    "presets": [
      "react-app"
    ],
    "plugins": [
      [
        "import",
        {
          "libraryName": "antd-mobile",
          "style": "css"
        }
      ],
      "transform-decorators-legacy"
    ]
  },
```

非eject模式下：

# 支持装饰器写法

* 找到`node_modules/babel-preset-react-app/index.js`，然后加入装饰器支持。

```js
const plugins = [
  require.resolve('babel-plugin-transform-decorators-legacy'),// 支持装饰器写法 增加此行
  // Necessary to include regardless of the environment because
  // in practice some other transforms (such as object-rest-spread)
  // don't work without it: https://github.com/babel/babel/issues/7215
  require.resolve('babel-plugin-transform-es2015-destructuring'),
  // class { handleClick = () => { } }
  require.resolve('babel-plugin-transform-class-properties'),
```

​

* 同目录下打开`package.json`文件，末尾添加

```
"babel":  {
    "presets": [
      "react-app"
    ],
    "plugins": [
      "transform-decorators-legacy"
    ]
}
```

6.`redux` 状态管理，提供`createStore`  `applyMiddleware` 和 `compose`

安装：

```
npm install redux —-save
```

7.`redux-thunk` ：中间件，使redux支持异步处理

安装：

```
npm install redux-thunk —-save
```

8.`redux-devtools-extension`：chrome支持redux插件，在chrome中安装Redux DevTool，之后需要添加配置， 使用compose结合thunk和window.devToolsExtension：

```js
const reduxDevTools = window.devToolsExtension ? window.devToolsExtension() : f =>f;

const store = createStore(reducers,compose(
    applyMiddleware(thunk),
    reduxDevTools
));
```

安装：

```
npm install redux-devtools-extension —-save
```

9.`react-redux`:提供`Provider`和`connect`两个接口，方便react和redux进行链接

* Provider组件在应用最外层，传入store即可，只用一次
* Connect负责从外部获取组件需要的参数

安装：

```
npm install react-redux —-save
```

10.`express`：基于nodejs，快速、开放、极简的web开发框架

* app.get、app.post 分别开发 get 和 post 接口
* app.use 使用模块
* 代 res.send（返回html）、res.json（返回json）、res.sendfile（） 响应不同的内容

安装：

```
npm install express —-save
```

11.`nodemon`：可以自动重启修改后的后端服务

安装:

```
npm install nodemon —-save
```

12.`mongoose`: 操作mongodb的驱动，即通过mongoose来操作mongodb，

mongoose的基础使用：

* Connect 链接数据库
* 定义文档模型，Schema 和 model 新建模型
* 代一个数据库文档对应一个模型，通过模型对数据库进行操作

mongoose的文档类型：

* String，Number 等数据结构
* create，remove , update 分别用来增、删、改的操作
* Find  和 findOne 用来查询数据

安装：

```
npm install mongoose —-save
```

13.`cookie-parser`:后端支持cookie的存储

```js
const bodyParser = require('body-parser’)；

app.use(bodyParser.json());
```

安装：

```
npm install cookie-parser —-save
```

14.`body-parser`: 解析post过来的json数据

```js
const bodyParser = require('body-parser’)；

app.use(bodyParser.json());
```

安装：

```
npm install body-parser —-save
```

15.`react-router-dom`:路由支持

安装：

```
npm install react-router-dom —-save
```

16.`utility`：密码加密库,可用于用户密码加密

```js
const utils = require('utility');

// md5
utils.md5('苏千').should.equal('5f733c47c58a077d61257102b2d44481');
utils.md5(new Buffer('苏千')).should.equal('5f733c47c58a077d61257102b2d44481');
// md5 base64 format
utils.md5('苏千', 'base64'); // 'X3M8R8WKB31hJXECstREgQ=='

// Object md5 hash. Sorted by key, and JSON.stringify. See source code for detail
utils.md5({foo: 'bar', bar: 'foo'}).should.equal(utils.md5({bar: 'foo', foo: 'bar'}))

// sha1
utils.sha1('苏千').should.equal('0a4aff6bab634b9c2f99b71f25e976921fcde5a5');
utils.sha1(new Buffer('苏千')).should.equal('0a4aff6bab634b9c2f99b71f25e976921fcde5a5');
// sha1 base64 format
utils.sha1('苏千', 'base64'); // 'Ckr/a6tjS5wvmbcfJel2kh/N5aU='

// Object sha1 hash. Sorted by key, and JSON.stringify. See source code for detail
utils.sha1({foo: 'bar', bar: 'foo'}).should.equal(utils.sha1({bar: 'foo', foo: 'bar'}));
```

安装：

```
npm install utility —-save
```

17.`prop-types`:对属性类型进行检查

安装：

```
npm install prop-types —-save
```

18.`browser-cookies`:浏览器端处理cookie

安装：

```
npm install browser-cookies —-save
```

19.socket.io:服务端支持websocket

安装：

```
npm install socket.io —-save
```

20.`socket.io-client`:客户端支持websocket

安装：

```
npm install socket.io-client —-save
```

21.`immutable`：数据创建后不会被改变，可用于react和redux的性能优化

优点：

* 减少内存使用
* 并发安全
* 降低项目复杂度
* 便于比较复杂数据，定制shouldComponentUpdate方便
* 时间旅行功能方便
* 函数式编程

缺点：

* 学习成本
* 库的大小
* 对原有项目入侵严重，所以新项目使用比较合适

安装：

```
npm install immutable —-save
```

22.`reselect：redux`数据优化缓存，reselect为selector设置了缓存，只有当selector的输入改变时，程序才重新调用selector函数。

使用：

```js
import {createSelector} from 'reselect';

// reselect是对redux进行优化的库
const numSelector = createSelector(
    state => state,
    // 第二个函数的参数，是第一个的返回值
    state => ({num:state*2})
);


/*
*  使用装饰器优化connect的代码
* */
@connect(
    state => numSelector(state),
    {addGun,removeGun,removeGunAsync,twiceGun}
```

安装：

```
npm install reselect —-save
```

23.`eslint`：可组装的JavaScript和JSX检查工具

`create-react-app`安装后有`eslint`模块，添加配置就在`eslintConfig`下进行添加，默认还扩展了react-app的配置。

`package.json`添加的额外配置的代号加上等级，等级有off（0）、warning（1）、error（2），代号在控制台上可以看到，也可以去官网上看到。

这些错误可以关闭，也可以进行修复。

```json
"eslintConfig": {
    "extends": "react-app",
    "rules": {
      "eqeqeq": [
        "off"
      ],
      "jsx-a11y/accessible-emoji": [
        0
      ]
    }
  },
```

24.`async+await`: 用同步方式写异步

```js
// 最原始的是使用callback，会陷入callback hell
// 然后是promise方案 es6
// 使用async和await来改变promise的写法  es7
export const readMsg = from => {
    // async和await写法
    return async (dispatch,getState) => {
        const res = await axios.post('/user/readmsg',{from});
        const userid = getState().user._id;
        if(res.status ==200 && res.data.code === 0){
            dispatch(msgRead({from,userid,num:res.data.num}));
        }
    }

    // promise写法
    return (dispatch,getState) =>{
        axios.post('/user/readmsg',{from})
            .then(res => {
                const userid = getState().user._id;
                if(res.status ==200 && res.data.code === 0){
                    dispatch(msgRead({from,userid,num:res.data.num}));
                }
            });
    }
}
```

25.`bundle-loader`:按需加载所需插件

安装：

```
npm install bundle-loader --save-dev
```

26.create-react-app支持`less`

首先安装所需插件

```
npm install less-loader less --save-dev
```

修改 `webpack.config.dev.js` 和 `webpack.config-prod.js` 配置文件

* `test: /\.css$/` 改为 `/\.(css|less)$/`
* `test: /\.css$/` 的 `use` 数组配置增加 `less-loader`

修改后如下：

```json
{
  test: /\.(css|less)$/,
  use: [
    require.resolve('style-loader'),
    {
      loader: require.resolve('css-loader'),
      options: {
        importLoaders: 1,
      },
    },
    {
      loader: require.resolve('postcss-loader'),
      options: {
        // Necessary for external CSS imports to work
        // https://github.com/facebookincubator/create-react-app/issues/2677
        ident: 'postcss',
        plugins: () => [
          require('postcss-flexbugs-fixes'),
          autoprefixer({
            browsers: [
              '>1%',
              'last 4 versions',
              'Firefox ESR',
              'not ie < 9', // React doesn't support IE8 anyway
            ],
            flexbox: 'no-2009',
          }),
        ],
      },
    },
    {
      loader:require.resolve('less-loader') // compiles Less to CSS
    }
  ],
},
```



27 `babel-node`:后台支持import语法

安装插件

```
npm install babel-node --save
```

使用：`package.json`修改`script`的server为最新的server

```json

"scripts": {
    ...
    "server": "NODE_ENV=test nodemon --exec babel-node server/server.js",
    "server_bak": "nodemon server/server.js"
  },

```

设备`node`的运行环境为`test`，即`NODE_ENV=test`，默认启用`node`，改为启用`babel-node`，再启动`server`。启用之后就会发现，后台也支持import语法了。

28 后端设置也支持jsx语法:

创建`.babelrc`，然后将package.json中的babel字段粘贴至此，使整个项目全局都支持此babel配置的语法

```json
{
  "presets": [
    "react-app"
  ],
  "plugins": [
    [
      "import",
      {
        "libraryName": "antd-mobile",
        "style": "css"
      }
    ],
    "transform-decorators-legacy"
  ]
}
```

