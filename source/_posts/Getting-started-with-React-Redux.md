---
title: Getting started with React Redux
layout: post
date: 2017-04-12 22:50:59
comments: true
tags: [Redux]
categories: [JavaScript]
keywords: react redux
description: how to start a project with React Redux
---

## Store

Store 就是保存数据的地方，你可以把它看成一个容器。
Redux提供`createStore`这个函数，用来生成Store。

```JavaScript
  import { createStore } from 'redux';
  const store = createStore(fn);

```

`createStore`函数接受另一个函数作为参数，返回新生成的Store对象。

### store.dispatch()

`store.dispatch()`是View发出Action的唯一方法。

```JavaScript
  import { createStore } from 'redux';
  const store = createStore(fn);

  store.dispatch({
    type: 'ADD_TODO',
    payload: 'Learn Redux'
  });

```

结合 Action Creator ，这段代码可以改写如下。
```JavaScript
  store.dispatch(addTodo('Learn Redux'));
```

<!-- more -->

### store.subscribe()

Store 允许使用 `store.subscribe` 方法设置监听函数，一旦 State 发生变化，就自动执行这个函数。

```JavaScript
  import { createStore } from 'redux';
  const store = createStore(reducer);

  store.subscribe(listener);
```

显然，只要把 View 的更新函数（对于 React 项目，就是组件的 `render` 方法或 `setState`方法）放入 `listen`，
就会实现 View 的自动渲染。

`store.subscribe`方法返回一个函数，调用这个函数就可以解除监听。

```JavaScript
  let unsubscribe = store.subscribe( () =>
    console.log(store.getState());
  );

  unsubscribe();
```

### store.getState()

  拿到拿钱时刻Store对应的State。

## State

`Store`对象包含所有数据。如果想要得到某个时点的数据，就要对Store生成快照。这种时点的数据集合，就叫做State。
当前时刻的State，可以通过`store.getState()`拿到。

```JavaScript
  import { createStore } from 'redux';
  const store = createStore(fn);

  const state = store.getState();
```

Redux规定，一个State对应一个View。只要State相同，View就相同。

## Action

State的变化，会导致View的变化。但是，用户接触不到State,只能接触到View。所以，State的变化必须是View导致的。
Action就是View发出的通知，表示State要发生怎样的变化。

Action是一个对象。其中的`type`属性是必须的，表示Action的名称。其他属性可以自由设置。
```JavaScript
  const action = {
    type: 'ADD_TODO',
    payload: 'Learn Redux'
  }
```

## Action Creator

View层要传递多少信息，就会有多少种Action。如果Action很多，可以定义一个函数来生成Action，这个函数就叫Action Creator。

```JavaScript
  const ADD_TODO = '添加TODO';

  function addTodo(text){
    return {
      type: ADD_TODO,
      text
    }
  }

  const action = addTodo('Learn Redux');
```

## Reducer

Store 收到 Action 以后，必须给出一个新的 State，这样View才会发生变化。这种State的计算过程就叫Reducer。
Reducer 是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State。

```JavaScript
  const reducer = function (state, action) {
    // ...
    return new_state;
  }
```

整个应用的初识状态，可以作为 State 的默认值。下面是一个实际的例子。

```JavaScript
  const defaultState = 0;
  const reducer = (state = defaultState, action) => {
    switch (action.type) {
      case 'ADD':
        return state + action.payload;
      default:
        return state;
    }
  };

  const state = reducer(1, {
    type: 'ADD',
    payload: 2
  });
```

在实际应用中，Reducer函数不用像上面这样手动调用，`store.dispatch` 方法会触发 Reducer 的自动执行。
为此，Store需要知道 Reducer 函数，做法就是在生成 Store 的时候，将 Reducer 传入 `createStore` 方法。

```JavaScript
  import { createStore } from 'redux';
  const store = createStore(reducer);
```

> 为什么这个函数叫 Reducer 呢？ 因为它可以作为数组 `reduce` 方法的参数。

```JavaScript
  const actions = [
    { type: 'ADD', payload: 0 },
    { type: 'ADD', payload: 1 },
    { type: 'ADD', payload: 2}
  ];

  const total = actions.reduce(reducer, 0); //3
```

## The Process

![](/images/getting-started-with-react-redux/redux-react.jpg)

![](/images/getting-started-with-react-redux/redux_flowchart.png)

![](/images/getting-started-with-react-redux/react-redux-introduction.jpg)
