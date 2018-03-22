# Apollo-Client简介

# Apollo-Client简介

### 一 、安装

在React中使用Apollo,首先需要安装相应插件：

```shell
# installing the preset package and react integration
npm install apollo-boost react-apollo graphql --save

# installing each piece independently
npm install apollo-client apollo-cache-inmemory apollo-link-http react-apollo graphql-tag graphql --save

```

React中使用Apollo，首先需要创建`ApolloClient`和`ApolloProvider`：

* `ApolloClient`是你应用程序中使用`Graphql`的中心，它管理你所有的`data`。
* `ApolloProvider`可以让你在应用中任何地方使用`Apollo`，就和`Redux`中的`Provider`一个道理，提供`context`，包裹在应用的最外层。

### 二、创建client和provider

1. 创建client作为客户端的graphql服务：

```js
import { ApolloClient } from 'apollo-client';
import { HttpLink } from 'apollo-link-http';
import { InMemoryCache } from 'apollo-cache-inmemory';

const client = new ApolloClient({
  // 默认情况下，客户端会讲查询发送至同域名下以`/graphql`为结尾的后端服务
  // 可以为new HttpLink()传入uri以制定后端服务地址，更多个性化配置可使用Apollo Link
  // link: new HttpLink({uri: 'http://localhost:4000/graphql'}),
  link: new HttpLink(),
  cache: new InMemoryCache()
});
```

ApolloClient还有很多其他的参数来控制客户端行为，可以去看看。

2. 创建Provider

   使用`ApolloProvider`组件来连接客户端至`Component Tree`。

   > We suggest putting the `ApolloProvider` somewhere high in your app, above any places where you need to access GraphQL data. For example, it could be outside of your root route component if you’re using React Router.

   ```js
   import { ApolloProvider } from 'react-apollo';
   import { ApolloClient } from 'apollo-client';
   import { HttpLink } from 'apollo-link-http';
   import { InMemoryCache } from 'apollo-cache-inmemory';

   const client = new ApolloClient({
     link: new HttpLink(),
     cache: new InMemoryCache()
   });

   ReactDOM.render(
     <ApolloProvider client={client}>
       <MyAppComponent />
     </ApolloProvider>,
     document.getElementById('root')
   )
   ```

### 三、使用graphql-tag创建操作语句

```js
import gql from 'graphql-tag';
```

`gql`模板标签是用来定义在apollo客户端的graphql查询。它会将`GraphQL query`解析成[GraphQL.js AST format](https://github.com/graphql/graphql-js/blob/d92dd9883b76e54babf2b0ffccdab838f04fc46c/src/language/ast.js) 。只要使用ql查询语句，都需要使用此标签进行包裹。

```js
onst query = gql`
    query Query($id:Int!){
      books(id:$id){
        title
        author
      }
    }
`;
```

### 四、使用graphql进行查询

API：`graphql(query, [config])(component)`

```js
import { graphql } from 'react-apollo';
```

这个API模式也在redux中的connect中使用，此方法会创建一个查询与更新Apollo Store的高阶组件。

1. query参数

```js
function TodoApp({ data: { todos } }) {
  return (
    <ul>
      {todos.map(({ id, text }) => (
        <li key={id}>{text}</li>
      ))}
    </ul>
  );
}

export default graphql(gql`
  query TodoAppQuery {
    todos {
      id
      text
    }
  }
`)(TodoApp);
```

2. config参数

>The `config` object is the second argument you pass into the `graphql()`function, after your GraphQL document. The config is optional and allows you to add some custom behavior to your higher order component.

```js
export default graphql(
  gql`{ ... }`,
  config, // <- The `config` object.
)(MyComponent);
```

​	`graphql`的第二个参数`config`有好6个，具体的可以在官网的[Query Configuration](https://www.apollographql.com/docs/react/basics/setup.html#graphql-config)看到，下面简单介绍下每个参数作用：

* `config.options` ：是一个对象或一个函数，在处理Graphql数据时，允许你的组件定义具体的行为。根据是`query`还是`mutation`，options下还有许多其他的参数，可以去详细了解。。。

  * 一个很经常用到的参数`options.variables`，给graphql语句中的变量传值。

    ```js
    export default graphql(gql`
      query ($width: Int!, $height: Int!) {
        ...
      }
    `, {
      options: (props) => ({
        variables: {
          width: props.size,
          height: props.size,
        },
      }),
    })(MyComponent);
    ```



*  `config.props`： 允许你定义一个映射函数，此函数可以使用你的props，包括query时的`props.data`和mutation时的`props.mutate`;并且，允许你返回一个新的props对象，此对象将会被提供给你所包裹的组件。我的理解就是，接收ql查询返回过来的props，并在将属性添加到包裹组件之前进行操作计算，也就是可以把组件中接收到props进行提前处理，直接将加工好的数据交给包裹组件，那么包裹的组件就不用再做其他计算，只负责实现UI就可以

  * This example uses [`props.data.fetchMore`](https://www.apollographql.com/docs/react/basics/setup.html#graphql-query-data-fetchMore). 加载更多，fetch

    ```js
    export default graphql(gql`{ ... }`, {
      props: ({ data: { fetchMore } }) => ({
        onLoadMore: () => {
          fetchMore({ ... });
        },
      }),
    })(MyComponent);

    function MyComponent({ onLoadMore }) {
      return (
        <button onClick={onLoadMore}>
          Load More!
        </button>
      );
    }
    ```

*  `config.skip` 它是一个布尔值，如果为`true`将会完全跳过此次查询。也可以是一个函数，但结果要返回一个布尔值。它的作用在于可以根据一些的props做出不同的查询

  ```js
  export default graphql(gql`{ ... }`, {
    skip: props => !!props.skip, // 查或者不查
  })(MyComponent);
  ```

  ​

* `config.name`： 默认情况下，query返回数据的`props`会命名为`data`，mutation返回数据的`props`会命名为`mutate`。此参数可更改返回的名字。当使用多个查询时，返回的props命名会冲突，后查询结果会覆盖前查询结构，为避免此状况发生，可以使用这个参数。

  栗子：

  ```js
  // 这里的compose和redux中的compose一样 具体可以往下看
  import { compose } from 'react-apollo';
  export default compose(
    graphql(gql`mutation (...) { ... }`, { name: 'createTodo' }),
    graphql(gql`mutation (...) { ... }`, { name: 'updateTodo' }),
    graphql(gql`mutation (...) { ... }`, { name: 'deleteTodo' }),
  )(MyComponent);

  function MyComponent(props) {
    // Instead of the default prop name, `mutate`,
    // we have three different prop names.
    console.log(props.createTodo);
    console.log(props.updateTodo);
    console.log(props.deleteTodo);

    return null;
  }
  ```

* `config.withRef` ：类似于React中的`ref`特性，具体可以自己去看。



### 五、compose作用简介

API：`compose(...enhancers)(component)`

```js
import { compose } from 'react-apollo';
```

使用compose可立马简洁优雅地使用一些组件增强剂。包括多个`graphql()`或Redux中的`connect()`。redux中也有一个compose，两者作用相同，用哪个都可以。

```js
export default compose(
  withApollo,
  graphql(`query { ... }`),
  graphql(`mutation { ... }`),
  connect(...),
)(MyComponent);
```

注意：`compose()`最先执行最后的`enhancer`。例子：`funcC(funcB(funcA(component)))`等价于`compose(funcC, funcB, funcA)(component)`

