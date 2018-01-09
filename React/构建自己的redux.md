# 构建自己的redux

### 前言：

`redux`的基本概念：

> [http://www.ruanyifeng.com/blog/2016/09/redux\_tutorial\_part\_one\_basic\_usages.html](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)

### 一、构建自己的redux

在redux中，对外主要提供`createStore`和`applyMiddleware`两种方法，具体的代码块如下：

创建`piggy-redux.js`

```js
// 片段一
export const createStore = (reducer,enhancer) => {
    // 如果有中间件，那么就用中间件包裹一层
    if(enhancer){
        return enhancer(createStore)(reducer);
    }

    let currentState = {};
    let currentListeners = [];

    const getState = () => currentState;
    const subscribe = listener => {
        currentListeners.push(listener);
    }
    const dispatch = action => {
        currentState = reducer(currentState,action);
        currentListeners.map(v => v());
        return action;
    }

    // 初始化的时候getState就能获取到值，先执行下dispatch
    dispatch({type:'@@piggy-redux'});
    return {getState,subscribe,dispatch};
}
```

redux提供`createStore`来返回一个对象，对象中包含的方法有：

* `getState`来获取此时的`state`状态
* `subscribe`来设置监听函数
* `dispatch`来发出`view`中的`action`到`reducer`，`dispatch`会自动操作`reducer`，所以`reducer`需要直接传进来；同时也调用了`subscribe`的函数，所以每次`dispatch`的时候被监听的函数都会被执行

```js
// 片段二 为actionCreater包裹一层dispatch  此方法是在react-redux中使用
// 将actionCreater包裹dispatch 以实现dispatch发送action
export const  bindActionCreaters = (creaters,dispatch) => {
    // 写法一
    // let bound = {};
    // Object.keys(creaters).forEach(v => {
    //    let creater = creaters[v];
    //    bound[v] = bindActionCreater(creater,dispatch);
    // })
    // return bound;

    // 写法二
    return Object.keys(creaters).reduce((ret,item) => {
        ret[item]  = bindActionCreater(creaters[item],dispatch);
        return ret;
    },{})
}

// 调用一个actioncreater函数如：addGun(…参数)，经过函数包裹后就是dispatch(addGun(...参数)) （…args）是参数透传，不理解creater中的参数怎么传过去的
const bindActionCreater = (creater,dispatch) => (...args) => dispatch(creater(...args))
```

```js
// 片段三 应用中间件
// 应用中间件
export const applyMiddleware = (...middleWares) => {
    return createStore => (...args) => {
        const store = createStore(...args);
        let dispatch = store.dispatch;

        const midApi = {
            getState:store.getState,
            dispatch:(...args) => dispatch(...args)
        }

        // 多个中间件
        const middleWareChain = middleWares.map(middleWare => middleWare(midApi))
        dispatch = compose(...middleWareChain)(store.dispatch);

        return {
            ...store,
            dispatch
        };
    }
}

const compose = (...funcs) => {
    if(funcs.length === 0) return func => func;
    if(funcs.length === 1) return funcs[0];
    return funcs.reduce((ret,item) => (...args) => ret(item(...args)))
}
```

### 二、构建react-redux

使用`react-redux`可以在react中优雅地使用redux。首先引入需要使用的类：

```js
import React from 'react';
import PropTypes from 'prop-types’;
import {bindActionCreaters} from './piggy-redux';
```

react-redux主要提供两个方法，

* 一个是Provider：把store放到context中，所有的子元素可以直接取到store；
* 另一个是connect：负责连接组件，将redux里的数据放到组件的属性里。作用是负责接受一个组件，将一些state放入进去；数据变化时，能够通知组件

```Js
// Provider
export class Provider extends React.Component{
    static childContextTypes = {
        store: PropTypes.object
    }

    getChildContext(){
        return {store:this.store};
    }

    constructor(props,context){
        super(props,context);
        this.store = props.store;
    }

    render(){
        return this.props.children;
    }
}
```

```js
export const connect = (mapStateToProps = state => state,mapDispatchToProps = {}) => WrapComponent => {
    return class ConnectComponent extends React.Component{
        static contextTypes = {
            store:PropTypes.object
        }
        constructor(props,context){
            super(props,context);
            this.state = {
                props:{}
            }
        }

        componentDidMount(){
            const {store} = this.context;
            // 在写createStore的时候一直不明白什么时候订阅的，是谁订阅的，写到这里突然明白了，是connect订阅的，确实很优雅
            store.subscribe(() => this.update())

            this.update();
        }

        update(){
            // 获取mapStateToProps和mapDispatchToProps将其更新到props中，使被connect包裹的组件可以直接使用store中的数据
            const {store} = this.context;
            const stateProps = mapStateToProps(store.getState());
            // state可以直接传入，但是action不能直接传入，action需要diapatch把它传给store。action的生成是通过acticnCreater生成的，所以用dispatch把actionCreater包裹一层即可
            // 将actionCreater包裹一层dispatch;同时函数内部也应该具有dispatch功能，所有把dispatch传进去  实现此功能的函数是redux给的，所以在redux中实现此函数
            const dispatchProps = bindActionCreaters(mapDispatchToProps,store.dispatch);
            this.setState({
                props:{
                    ...this.state.props,
                    ...stateProps,
                    ...dispatchProps
                }
            });
        }

        render(){
            return <WrapComponent {...this.state.props}></WrapComponent>
        }
    }
}
```

### 三、使用它们

两个简单的中间件

```js
// 便于理解上面的中间件应用
const thunk  = ({dispatch,getState}) => next => action => {
    if(typeof action === 'function'){
        return action(dispatch,getState);
    }


    return next(action);
}

export default thunk;

/*--------------------------------------------*/
const arraythunk  = ({dispatch,getState}) => next => action => {
    if(Array.isArray(action)){
        // 如果是数组，遍历dispatch出数据
        return action.forEach(v => dispatch(v))
    }

    return next(action);
}

export default arraythunk;
```

使用：

```js
// action type
const ADD_GUN = 'ADD_GUN';
const REMOVE_GUN = 'REMOVE_GUN';

// reducer
export const counter = (state = 0,action) => {
    switch (action.type){
        case 'ADD_GUN':
            return state+1;
        case 'REMOVE_GUN':
            return state-1;
        default:
            return 10;
    }
}

// action creater
export const addGun = () => {
    return {type:ADD_GUN}
}
export const  removeGun = () => {
    return {type:REMOVE_GUN}
}

export const twiceGun = () => {
    return [{type:ADD_GUN},{type:ADD_GUN}];
}

export const removeGunAsync = () => (
    dispatch => setTimeout(() => {
        dispatch(removeGun())
    },2000)
)
```

```js
import React from 'react';
import {connect} from './piggy-react-redux'
import {addGun,removeGun,removeGunAsync,twiceGun} from './react.redux'
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
)

class App extends React.Component{

    render(){
        return(
            <div>

                <h1>现在有{this.props.num}把枪</h1>
                <button onClick={this.props.addGun}>申请武器</button>
                <button onClick={this.props.removeGun}>上交武器</button>
                <button onClick={this.props.removeGunAsync}>等会上交</button>
                <button onClick={this.props.twiceGun}>申请两把</button>
            </div>
        );
    }
}

// const mapStateToProps = (state) => {
//     return {
//         num : state
//     }
// }

// 如果action的方法名和在组件中使用的名称不一致，可以这样写，不过一般情况下都是保持一致的
// const mapDispatchToProps = (dispatch) => {
//     return {
//         addGun: () => {
//             dispatch(addGun())
//         },
//         removeGun: () => {
//             dispatch(removeGun())
//         },
//         removeGunAsync: () => {
//             dispatch(removeGunAsync())
//         }
//     }
// }

// 如果一致，就可以进行简写
// const mapDispatchToProps = {addGun,removeGun,removeGunAsync};

/*
* connect接收两个参数，一个mapStateToProps,就是把redux的state，
* 转为组件的Props，还有一个参数是mapDispatchToprops,就是把
* dispatch(actions)的方法，转为Props属性函数。
* 也就是说，第一个参数是：你要state中的什么属性放到组件的props中
* 第二个参数是：你要actions中的什么方法放到组件的props中，并自动dispatch
*
* 下面的connect写法是标准的装饰器写法，就是connect先执行，然后再把App
* 当做参数传进来
* */
// App = connect(mapStateToProps,mapDispatchToProps)(App);


export default App;
```



