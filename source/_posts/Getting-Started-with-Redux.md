---
title: Getting Started with Redux
layout: post
date: 2017-08-13 18:52:40
comments: true
tags: [Redux]
categories: [JavaScript]
keywords: react redux
description: how to start a project with React Redux
---


## Redux的需求从何而来？

### React中的props和state

    a. props代表父级分发到子级的属性

    b. state代表组件内部的状态

  React没有数据向上回溯的能力，也就是说只能由父级到子级向下分发，或者内部消化。

### React中两个组件的交互

  一般一个组件内部就可能是一个完整的应用，你可以通过属性作为API来控制它。但是更多时候React无法让两个组件相互交流，使用对方的数据。然后这时候如果不通过DOM，唯一的解决办法就是提升state。将state放到共有的父组件中来管理，再通过props分发到子组件。子组件改变父组件state的办法通常是通过onClick触发父组件声明好的回调，这就需要父组件提前生命好函数或者方法去约定state的变化。这样就出现了一个模式：数据总是单向从顶层向下分发的，但是只有子组件回调时，会影响顶层的数据，这样的state在一定程度上是响应式的。

  为了面临所有可能性的扩展，最容易想到的办法就是把state集中到所有组件的顶层管理，然后发给子组件。

### Redux解决的问题

  为了有更好的state管理，就需要一个库来作为更专业的顶层state分发给所有的React应用，这就是Redux。

  重现上面结构的需求：

    a. 需要回调通知state(等同于回调函数) => Action

    b. 需要根据回调处理（等同于父级方法）=> Reducer

    c. 需要state(等同于总状态) => store

<!-- more -->    

### Redux中三个重要要素

  * Action是纯声明式的数据结构，只提供事件的所有要素，不提供逻辑。

  * Reducer是一个匹配函数，Action的发送是全局的: 所有的reducer都可以捕捉到并匹配是否与自身相关，能够匹配上就拿走Action中的要素进行逻辑处理，修改store中的状态，不匹配就不对state进行处理原样返回。

  * store负责存储状态并可以被react api回调，发布action。

## Redux

### Action

  Action是把数据从应用（服务器响应、用户输入或者其他非view的数据）传到store的有效载荷。它是store数据的`唯一`来源。一般来说你会通过`store.dispatch()`将action传入store。

  Action是一个对象，其中的`type`属性是必须的，表示Action的名称，其他属性可以自由设置。

  添加新的 todo 任务的 action 是这样的 :

  ```javascript
    const ADD_TODO = 'ADD_TODO';

    type: ADD_TODO,
    text: 'Build my first Redux app'
  ```

### Reducer

    Action只是描述了`有事情发生了`这一事实，并没有指明应用如何更新state。而这正是`reducer`要做的事情。

    Store 收到 Action以后，必须给出一个新的State，这样 View 才会发生变化。这种State的计算过程就叫Reducer。

    Reducer是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State。

    ```javascript
      function todoApp(state = initialState, action) {
        switch (action.type) {
          case SET_VISIBILITY_FILTER:
            return Object.assign({}, state, {
                visibilityFilter: action.filter
              })
          default:
            return state
        }
      }
    ```

    > 注意

    * 不要修改`state`。使用`Object.assign()`新建一个副本，不能使用 `Object.assign(state, { visibilityFilter: action.filter })`, 因为它会改变第一个参数的值，你`必须`把第一个参数设置为空对象。

    * 在 `default` 情况下返回旧的 `state`。遇到位置的 action 时，一定要返回旧的 `state`。

### Store

  我们知道 `action` 来描述“发生了什么”，使用 `reducer`来根据 action 更新 state的用法。

  `Store` 就是把它们联系到一起的对象。Store有一下职责：

  * 维持应用的 state；
  * 提供 `getState()` 方法获取 state;
  * 提供 `dispatch(action)` 方法更新 state;
  * 通过 `subscribe(listener)` 注册监听器；
  * 通过 `subscribe(listener)` 返回的函数注销监听器。

  Redux提供`createStore`这个函数，用来生成Store。

  ```javascript
    import { createStore } from 'redux';
    import todoAPP from './reducers';
    let store = creacteStore(todoApp);
  ```

### 数据流

      1. 调用 `store.dispatch(action)`
      2. Redux store 调用传入的 reducer 函数
      3. 根 reducer 把多个子 reducer 输出合并成一个单一的 state 树
      4. Redux store 保存了根 reducer 返回的完整 state 树


  ![](/images/getting-started-with-redux/redux.png)
  ![](/images/getting-started-with-redux/redux-workflow.png)

> view ---> action ---> reducer ---> store(state) ---> view  

## React-Redux

![](/images/getting-started-with-redux/react-redux.png)

### UI组件

  * 只负责 UI 的呈现，不带有任何业务逻辑
  * 没有状态（即不使用this.state这个变量）
  * 所有数据都由参数（this.props）提供
  * 不使用任何 Redux 的 API

### 容器组件

  * 负责管理数据和业务逻辑，不负责 UI 的呈现
  * 带有内部状态
  * 使用 Redux 的 API

### Provider

  Provider是一个普通组件，可以作为顶层app的分发点，它只需要store属性就可以了。它会将state分发给所有被connect的组件，不管它在哪里，被嵌套多少层。

  React-Redux 提供Provider组件，可以让容器组件拿到state。

  ```javascript

    import { Provider } from 'react-redux'
    import { createStore } from 'redux'
    import todoApp from './reducers'
    import App from './components/App'

    let store = createStore(todoApp);

    render(
    <Provider store={store}>
      <App />
    </Provider>,
    document.getElementById('root')
    )

  ```

### connect

  connect是真正的重点，它是一个科里化函数，意思是先接受两个参数（数据绑定mapStateToProps和事件绑定mapDispatchToProps），再接受一个参数（将要绑定的组件本身）。

  * mapStateToProps

  建立一个从（外部的）state对象到（UI 组件的）props对象的映射关系。

  作为函数，mapStateToProps执行后应该返回一个对象，里面的每一个键值对就是一个映射。

  ```javascript

    const mapStateToProps = (state) => {
      return {
        todos: getVisibleTodos(state.todos, state.visibilityFilter)
      }
    }

  ```

  * mapDispatchToProps

  mapDispatchToProps是connect函数的第二个参数，用来建立 UI 组件的参数到store.dispatch方法的映射。也就是说，它定义了哪些用户的操作应该当作 Action，传给 Store。它可以是一个函数，也可以是一个对象。

  ```javascript

    const mapDispatchToProps = (
      dispatch,
      ownProps
    ) => {
      return {
        onClick: () => {
          dispatch({
            type: 'SET_VISIBILITY_FILTER',
            filter: ownProps.filter
          });
        }
      };
    }

  ```

## React-Router

用React-Router的项目，与其他项目没有不同之处，也是使用Provider在Router外面包一层，毕竟Provider的唯一功能就是传入store对象。

```javascript

  const Root = ({ store }) => (
    <Provider store={store}>
      <Router>
        <Route path="/" component={App} />
      </Router>
    </Provider>
  );

```  


## Resource

> Document

* http://cn.redux.js.org/index.html

* https://alisec-ued.github.io/2016/11/23/%E5%9B%BE%E8%A7%A3Redux%E6%95%B0%E6%8D%AE%E6%B5%81(%E4%B8%80)/

> Video

* https://egghead.io/lessons/javascript-redux-the-single-immutable-state-tree
