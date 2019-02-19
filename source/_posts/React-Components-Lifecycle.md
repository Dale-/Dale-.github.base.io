---
title: React Components Lifecycle
date: 2017-12-4 22:05:49
tags: [React]
categories: [React]
keywords: Components-Lifecycle
description: React Components Lifecycle
---

## 虚拟DOM

React对底层的代码作了封装，在大多数情况下，我们不需要直接去操作DOM。但是有时候我们还是需要使用到底层的代码的，比如输入框获取焦点，这个时候可以通过第三方的类库或者React提供的API实现。

React之所以快，是因为它不直接操作DOM。React将DOM结构存储在内存中，然后同render()的返回内容进行比较，计算出需要改动的地方，最后才反映到DOM中。
此外，React实现了一套完整的事件合成机制，能够保持事件冒泡的一致性，跨浏览器执行。甚至可以在IE8中使用HTML5的事件。
大部分情况下，我们都是在构建React的组件，也就是操作虚拟DOM。但是有时候我们需要访问底层的API，可能或通过使用第三方的插件来实现我们的功能，如jQuery。React也提供了接口让我们操作底层API。

### Virtual DOM 算法步骤

* 用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中
* 当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异
* 把2所记录的差异应用到步骤1所构建的真正的DOM树上，视图就更新了

Virtual DOM 本质上就是在 JS 和 DOM 之间做了一个缓存。可以类比 CPU 和硬盘，既然硬盘这么慢，我们就在它们之间加个缓存：既然 DOM 这么慢，我们就在它们 JS 和 DOM 之间加个缓存。CPU（JS）只操作内存（Virtual DOM），最后的时候再把变更写入硬盘（DOM）。

## 生命周期的三种状态

![](/images/react-components-lifecycle/component-lifecycle.png)

### Mounted

React Components被render解析生成对应的DOM节点并被插入浏览器的DOM结构的一个过程。

### Update

一个mounted的React Components被重新render的过程。

### Unmounted

一个mounted的React Components对应的DOM节点被从DOM结构中移除的这样一个过程。

<!-- more -->

## 生命周期

React 为每个状态都提供了两种处理函数，will 函数在进入状态之前调用，did 函数在进入状态之后调用，三种状态共计五种处理函数。

* componentWillMount()
* componentDidMount()
* componentWillUpdate(object nextProps, object nextState)
* componentDidUpdate(object prevProps, object prevState)
* componentWillUnmount()

此外，React 还提供两种特殊状态的处理函数。

* componentWillReceiveProps(object nextProps)：已加载组件收到新的参数时调用
* shouldComponentUpdate(object nextProps, object nextState)：组件判断是否重新渲染时调用

### Mounting
![](/images/react-components-lifecycle/mounting.png)
<script async src="//jsfiddle.net/Dale_/gfLrfg1d/embed/"></script>

### Updating
![](/images/react-components-lifecycle/updating-props.png)
![](/images/react-components-lifecycle/updating-state.png)
<script async src="//jsfiddle.net/Dale_/gfLrfg1d/2/embed/"></script>

### Unmounting
![](/images/react-components-lifecycle/unmounting.png)

## 生命周期API
![](/images/react-components-lifecycle/cycle-react.png)

### getDefaultProps
```javascript
  Counter.defaultProps = { initialCount: 0 };
```
只在组件创建时调用一次并缓存返回的对象（即在 React.createClass 之后就会调用）。

因为这个方法在实例初始化之前调用，所以在这个方法里面不能依赖 this 获取到这个组件的实例。
在组件装载之后，这个方法缓存的结果会用来保证访问 this.props 的属性时，当这个属性没有在父组件中传入（在这个组件的 JSX 属性里设置），也总是有值的。
如果是使用 ES6 语法，可以直接定义 defaultProps 这个类属性来替代，这样能更直观的知道 default props 是预先定义好的对象值：


### getInitialState
```javascript
  class Counter extends Component {
    constructor(props) {
      super(props);
      this.state = { count: props.initialCount };
    }

    render() {
      // ...
    }
  }
```
初始化 this.state 的值，只在组件装载之前调用一次。
如果是使用 ES6 的语法，你也可以在构造函数中初始化状态，比如：


### componentWillMount
只调用一次，在客户端与服务端都执行。在初始化渲染之前被调用

### render
必选的方法，创建虚拟DOM，该方法具有特殊的规则：

* 只能通过this.props和this.state访问数据
* 可以返回null、false或任何React组件
* 只能出现一个顶级组件（不能返回数组）
* 不能改变组件的状态
* 不能修改DOM的输出

### componentDidMount
真实的DOM被渲染出来后调用，在该方法中可通过this.getDOMNode()访问到真实的DOM元素。此时已可以使用其他类库来操作这个DOM。

只调用一次，在客户端执行，不在服务端执行。在初始化渲染之后被调用。
使用 `setTimeout` 或 `setInterval`, Ajax 请求等这些操作，均在这个方法内。

### componentWillReceiveProps

```javascript
  componentWillReceiveProps (nexProps) {
    if (!this.props.application && nexProps.application) {
      this.openReminderDialog();
    }
  }
```

组件接收到新的props时调用，并将其作为参数nextProps使用，此时可以更改组件props及state。

不会在初始化渲染被调用。在这个函数里调用 this.setState()不会触发任何额外的渲染。


### shouldComponentUpdate

```javascript
  shouldComponentUpdate (nextProps, nextState) {
    const currProps = this.props;
    const currState = this.state;

    if (!currProps.position && nextProps.position) return true;
    if (currProps.position && !currProps.position.company && nextProps.position.company) return true;
    if (!currProps.application && nextProps.application) return true;
    if (currProps.user !== nextProps.user) return true;

    if (currState.showInfo !== nextState.showInfo) return true;
    if (currState.showConfirm !== nextState.showConfirm) return true;

    return false;
  }
```

Use `shouldComponentUpdate()` to let React know if a component’s output is not affected by the current change in state or props. The default behavior is to re-render on every state change, and in the vast majority of cases you should rely on the default behavior.

`shouldComponentUpdate()` is invoked before rendering when new props or state are being received. Defaults to `true`. This method is not called for the initial render or when `forceUpdate()` is used.

Returning `false` does not prevent child components from re-rendering when their state changes.

Currently, if `shouldComponentUpdate()` returns `false`, then `UNSAFE_componentWillUpdate()`, `render()`, and `componentDidUpdate()` will not be invoked. Note that in the future React may treat `shouldComponentUpdate()` as a hint rather than a strict directive, and returning `false` may still result in a re-rendering of the component.

If you determine a specific component is slow after profiling, you may change it to inherit from `React.PureComponent` which implements `shouldComponentUpdate()` with a shallow prop and state comparison. If you are confident you want to write it by hand, you may compare `this.props` with `nextProps` and `this.state` with `nextState` and return `false` to tell React the update can be skipped.

We do not recommend doing deep equality checks or using `JSON.stringify()` in `shouldComponentUpdate()`. It is very inefficient and will harm performance.

`shouldComponentUpdate()`发生在重新渲染页面之前，当有新的`props`或者`state`被接受时被调用。默认返回`true`。此方法不会再初始化(initial render)的时候被调用，除非使用`forceUpdate()`时。

组件是否应当渲染新的`props`或`state`，返回false表示跳过后续的生命周期方法，通常不需要使用以避免出现bug。在出现应用的瓶颈时，可通过该方法进行适当的优化。

在首次渲染期间或者调用了forceUpdate方法后，该方法不会被调用


### componentWillUpdate
接收到新的props或者state后，进行渲染之前调用，此时不允许更新props或state。

### componentDidUpdate
完成渲染新的props或者state后调用，此时可以访问到新的DOM元素,不会在初始化渲染被调用。

### componentWillUnmount
在组件卸载前被调用，主要用来执行一些组件的清理工作。

<script async src="//jsfiddle.net/Dale_/sttnf78z/9/embed/"></script>

[React Component Lifecycle](https://reactjs.org/docs/react-component.html)

## key的作用
> Key is not really about performance, it’s more about identity (which in turn leads to better performance). Randomly assigned and changing values do not form an identity

react中的key属性，它是一个特殊的属性，它是出现不是给开发者用的（例如你为一个组件设置key之后不能获取组件的这个key props），而是给react自己用的。

简单来说，react利用key来识别组件，它是一种身份标识标识，就像我们的身份证用来辨识一个人一样。每个key对应一个组件，相同的key react认为是同一个组件，这样后续相同的key对应组件都不会被创建。例如下面代码：

```javascript
  const UserList = props => (
    <div>
      <h3>User List</h3>
      {props.users.map(u => <div key={u.id}>{u.id}:{u.name}</div>)}  
    </div>
  );
```
这样，有了key属性后，就可以与组件建立了一种对应关系，react根据key来决定是销毁重新创建组件还是更新组件。

* key相同，若组件属性有所变化，则react只更新组件对应的属性；没有变化则不更新。
* key值不同，则react先销毁该组件(有状态组件的componentWillUnmount会执行)，然后重新创建该组件（有状态组件的constructor和componentWillUnmount都会执行）
